---
name: ship-pr
description: Use when the user is done editing and wants to ship the current branch via a pull request — phrases like "open a PR", "ship it", "ship this", "submit the PR", "let's send it", "提 PR", "提个 PR", "提个 MR". Walks the canonical post-edit flow for any GitHub repo: confirm the branch → push → open the PR (respecting the repo's template) → wait for CI → review the diff → triage findings (self-fix small stuff, escalate real decisions, merge directly when clean) → local cleanup. Trigger whenever the user signals a change is finished and should head to the default branch, even if they don't say the word "PR". Do NOT trigger for `gh pr view` / status checks on an existing PR, for draft / WIP PRs the user wants opened but not merged, or for cutting a release tag.
license: MIT
metadata:
  category: git
  language: en
---

# Ship a PR

The canonical way to move a finished change from a feature branch to the
repo's default branch through a pull request — for **any** GitHub repo,
without hardcoding one project's build commands or check names.

There are two gates between your branch and the default branch:

- **Mechanical** — the repo's required CI checks. GitHub already enforces
  these via branch protection; let them run.
- **Inferential** — a human-judgment review of the diff. CI catches what
  it knows how to check; this catches behaviour, design, test coverage,
  risk, and rollback. This gate is the reason the skill exists.

**Auto-merge is intentionally not used.** `gh pr merge --auto` merges the
moment CI passes, which skips the review gate entirely — that defeats the
purpose. The merge command is run from this session, *after* the diff has
been reviewed.

## When NOT to use

- **Draft / WIP PRs** the user wants reviewed but not merged → just
  `gh pr create --draft` and stop.
- **Changes to `.github/workflows/`, branch protection, or repo settings**
  → these usually need a human in the GitHub UI too; flag it.
- **Cutting a release** (tags like `v1.2.3`) → releases follow the repo's
  own release process, not this flow.

## The flow

### Step 1 — Confirm the branch is shippable

```bash
git status -sb                                # clean except the expected diff?
git rev-parse --abbrev-ref HEAD               # the current branch
git fetch origin --quiet                      # refresh the remote base ref
BASE=$(git symbolic-ref --quiet --short refs/remotes/origin/HEAD 2>/dev/null | sed 's@^origin/@@')
BASE=${BASE:-main}                            # the repo's default branch
git log --oneline "origin/$BASE..HEAD"        # commits actually exist
```

`$BASE` is the repo's default branch — usually `main`, sometimes `master`
or `develop`. Don't assume `main`; use what was detected. Comparisons run
against the **remote** ref `origin/$BASE` (hence the `git fetch`), so they
match what GitHub will diff the PR against even when the local base branch
is missing or stale — common in worktrees and feature-only checkouts.

- On the base branch → **stop**, make a feature branch first.
- No commits ahead of `origin/$BASE` → **stop**, nothing to ship.
- A PR may already exist for this branch (`gh pr view` succeeds) → skip
  Step 3, go straight to Step 4.

### Step 2 — Push

```bash
git push -u origin "$(git rev-parse --abbrev-ref HEAD)"
```

If the repo installs a `pre-push` hook (Husky, `core.hooksPath`, etc.) it
runs here automatically — don't re-run the same lint/test suite by hand.
If no hook is installed, that's the user's setup; CI is still the gate.

### Step 3 — Open the PR

Check for a PR template first — using it keeps the PR consistent with the
repo's conventions instead of inventing a different shape:

```bash
ls .github/pull_request_template.md \
   .github/PULL_REQUEST_TEMPLATE.md \
   .github/PULL_REQUEST_TEMPLATE/ 2>/dev/null
```

If a template exists, fill in its sections honestly — including any
cross-repo / checklist items (e.g. "needs a docs update?"). Do **not**
discard it by passing a wholly custom `--body`.

If there's no template, fall back to a sensible default:

```bash
gh pr create --title "<conventional commit subject>" --body "$(cat <<'EOF'
## Summary

- <what changed and why>

## Test plan

- [ ] <how you verified>
EOF
)"
```

Title rules:

- Conventional Commits prefix: `feat:` / `fix:` / `docs:` / `refactor:` /
  `test:` / `chore:` / `ci:` / `perf:` / `style:` / `build:` / `revert:`.
- Optional scope: `fix(api):`, `feat(editor):`.
- Keep under ~70 chars. Detail goes in the body.

### Step 4 — Wait for CI

```bash
gh pr checks --watch
```

This blocks until every check finishes. If a **required** check fails:

- **Stop. Do not proceed to review or merge.**
- Read the failure (`gh run view --log-failed`), explain it to the user,
  and ask whether to fix it now.
- Checks that aren't required (drift sensors, optional coverage, advisory
  scans) are informational — flag them, but they don't block the merge.

If you can't tell which checks are required, the ones branch protection
enforces are; `gh pr checks` marks the rest, and the merge in Step 7 will
refuse anyway if a required one is red.

### Step 5 — Review the diff

Once CI is green, review the **full PR diff** (`git diff "origin/$BASE"...HEAD`).
This is the inferential gate.

- If a `/code-review` command or a repo-specific review skill is
  available, run it on the full diff — it's purpose-built for this.
- Otherwise review inline: behaviour, design, test coverage for the
  branches this PR adds, risk, and how it rolls back.

### Step 6 — Triage the findings

Every finding lands in exactly one bucket. Getting this right is what
keeps the gate from failing open.

**Self-fixable → fix now, then loop back to Step 4.** Mechanical
corrections clearly inside the PR's stated scope: typos, formatting, doc
wording, dead code this PR introduced, missing imports, missing tests for
branches this PR adds, bug fixes that don't change observable behaviour,
or following through on a rule the user already stated this session. Fix
it, push to the same branch, wait for CI again, then re-review. Don't
prompt the user — the point is to spend their attention only on real
decisions.

**Needs user judgment → surface and stop.** Anything with a genuine
choice: design / API shape / behaviour changes, anything touching a
deliberate prior decision or an existing convention, anything that would
expand the PR beyond its stated scope — anything you'd ask a teammate
about before pushing. State the file/line, the option, and the question;
stop the flow.

**Clean → merge directly.** If CI is green AND nothing is self-fixable
AND nothing needs judgment, go straight to Step 7. Don't ask "want me to
merge?" — asking when there's nothing to decide just burns attention. The
loop is supposed to close itself.

Rule of thumb: would a thoughtful engineer file this as a question, push
a follow-up commit, or just merge it? Escalate / fix / merge accordingly.

### Step 7 — Merge

Reached on a clean review, or after the user approved a merge following an
escalation.

```bash
gh pr merge --squash --delete-branch
```

- **No `--auto`** — it skips the review gate (Step 5).
- **No `--admin`** — it bypasses branch protection.
- `--squash` keeps the default branch at one commit per PR; if the repo
  only allows merge commits or rebase, use `--merge` / `--rebase` instead
  (`gh` errors and tells you if the method isn't enabled).
- Branch protection still applies — if something flipped red since Step 4,
  GitHub refuses and you loop back to Step 4.

Report the result as a one-liner ("PR #N merged, branch deleted, local
synced"). On a clean review, don't ask for confirmation *before* merging.

### Step 8 — Local cleanup

`--delete-branch` removed the remote branch. Sync local back to base:

```bash
BASE=$(git symbolic-ref --quiet --short refs/remotes/origin/HEAD 2>/dev/null | sed 's@^origin/@@')
git checkout "${BASE:-main}" && git pull --ff-only
git branch -d "$(git rev-parse --abbrev-ref @{-1})"
```

(`@{-1}` is the previously checked-out branch — the one just merged. Use
`git rev-parse --abbrev-ref`, not `git symbolic-ref`, which rejects the
`@{-1}` checkout-history shorthand.) This
is part of the loop, not optional: because the merge is synchronous (no
`--auto`), there's no reason to leave a merged feature branch checked out.

## What NOT to do

- **No `--auto`** — skips the review gate, the whole reason for this skill.
- **No `--admin`** — bypasses branch protection.
- **Don't merge before CI finishes** — even a trivial-looking diff; drift
  checks sometimes catch surprising things.
- **Don't amend / force-push after `gh pr create`** unless the user asks —
  it invalidates in-flight reviews and re-runs CI from scratch.
- **Don't push directly to the default branch** — branch protection
  rejects it, and it skips both gates.
- **Don't auto-fix findings that need judgment** — behaviour changes,
  scope expansion, or anything touching a prior deliberate choice gets
  surfaced in Step 6, not silently committed. Self-fixable nits are the
  exception, not the default.
