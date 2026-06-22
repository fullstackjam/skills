# fullstackjam/skills

Personal Claude Code skill marketplace for fullstackjam.

Install as a Claude Code plugin marketplace:

```text
/plugin marketplace add fullstackjam/skills
/plugin install fullstackjam@skills
```

After installation, Claude Code can discover and invoke any skill under `skills/`.

## Get updates

This marketplace is served from this git repo, so updates are pulled, not pushed.
When a new version is published, refresh the marketplace catalog and then update
the plugin:

```text
/plugin marketplace update skills      # re-fetch repo metadata
/plugin update fullstackjam@skills     # update the installed plugin
```

`skills` is the marketplace name; `fullstackjam` is the plugin name. You can also
open the `/plugin` menu and update from there.

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

- `op-vault` — 从 1Password 取凭据，注入到要跑的命令里运行（`op run`），密钥明文不进对话 /
  输出。检测到命令缺密码 / token / API key 时自动介入，也可点名 vault（"dev vault"）。
  需要装了 `op` CLI 且开启 1Password 桌面 App 集成。

```text
/fullstackjam:op-vault 用 dev vault 的数据库密码跑 psql
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
