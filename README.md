# fullstackjam/skills

Personal Claude Code skill marketplace for fullstackjam.

Install as a Claude Code plugin marketplace:

```text
/plugin marketplace add fullstackjam/skills
/plugin install fullstackjam@skills
```

After installation, Claude Code can discover and invoke any skill under `skills/`.

## Skills

- `sound-like-me` — write, rewrite, and polish any outgoing text (blog posts,
  docs, README, notes, commits, chat messages) in fullstackjam's personal voice.
  Handles both Chinese and English, stripping AI-ese and translationese.

```text
/fullstackjam:sound-like-me 帮我把这段笔记整理成博客
/fullstackjam:sound-like-me 按我的风格润色这段文字
/fullstackjam:sound-like-me rewrite this so it doesn't sound like AI
/fullstackjam:sound-like-me 这段像不像我的写法
```

## Add a skill

Create `skills/<name>/SKILL.md` with frontmatter:

```md
---
name: <name>
description: What this skill does and when to use it.
---
```

Use a kebab-case `name` that matches its directory.

## Layout

```text
.claude-plugin/   marketplace.json + plugin.json
skills/           one folder per skill, each with a SKILL.md
```
