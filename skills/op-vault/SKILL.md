---
name: op-vault
description: 当 AI agent 需要凭据（密码 / token / API key / 连接串）来跑命令时，从 1Password 取出对应 item 并以环境变量注入命令运行，密钥明文不进对话或输出。检测到命令缺密码、token、API key、连接串，或环境变量名形如 *_PASSWORD / *_TOKEN / *_API_KEY 时触发；也可用昵称点名 vault（"dev vault" / "work vault" / "private vault"）或说"用 1Password 里的 X 跑 Y"。Use when an agent needs a secret from 1Password to run a command. 需要装了 op CLI 且开启 1Password 桌面 App 集成；不做密钥的存储 / 创建 / 轮换，只读取与注入。
license: MIT
metadata:
  category: secrets
  language: zh-CN, en
---

# op-vault

从 1Password 取凭据，注入到要跑的命令里，密钥明文不落进 agent 的对话 / 输出。底层全是
`op` CLI，这层封装只省去手敲 `op://` 路径，并守住一条线：**别让明文落进会被持久化的输出**。
人自己敲 `op` 不需要它，直接 `op run` 就行。

## 何时用

- agent 要跑的命令缺凭据才能继续（缺密码 / token / API key / 连接串，或环境变量名形如
  `*_PASSWORD` / `*_TOKEN` / `*_API_KEY`）。
- 用户点名 vault（"dev vault"）、说"用 1Password 里的 X 跑 Y"。

默认**先查后问**：先去 1Password 找；有把握的匹配，先说用的是哪个 item 再注入；找不到或有
歧义才问用户。绝不拿猜的凭据静默注入。

## 流程

1. **鉴权**：`op whoami`。报错就让用户解锁 1Password 桌面 App（必要时 `op signin`），然后停。
2. **找 vault / item**：
   - 有 vault 线索：`op vault list --format json` 模糊匹配昵称。
   - 找 item：`op item list --vault <V> --format json`，按服务名 / 标题模糊匹配；没把握就把
     标题列给用户选。
3. **看字段（只读元数据，关键）**：`op item get <ITEM> --vault <V> --format json`，**不要加
   `--reveal`，也不要用 `op read` 去"看一眼"**。从返回里读字段 label，确认
   `username` / `password` / `credential` 等哪些存在。带 concealed 的值会以掩码返回——这正
   是要的。
4. **定环境变量名**：默认字段 label 转大写（`username` → `USERNAME`）；用户能临时覆盖
   （"as PGUSER/PGPASSWORD"）。
5. **注入并执行**（见下）。密钥不打印，只回报退出码和命令输出。

## 注入（默认一种）

```bash
USERNAME="op://<Vault>/<Item>/username" PASSWORD="op://<Vault>/<Item>/password" op run -- <命令>
```

明文只在 `op run` 子进程里短暂存在，不进当前 shell；命令行里只有 `op://` 引用；`op run`
默认掩码 stdout/stderr。

### 少数例外

- **命令里要引用注入的变量**（如 `$PASSWORD`）：包一层 `op run -- sh -c '...'`（单引号），
  让 `$PASSWORD` 在子进程里展开，否则会在外层 shell 展开成空值。
- **交互式工具**（psql / mysql 的 REPL、ssh）：`op run` 会影响终端 → 改用内联
  `VAR="$(op read 'op://...')" <命令>`。
- **密钥只能当命令行参数传、又没有 `--password-stdin` 之类选项**：别硬塞（会进 `ps`），
  告诉用户这个工具没有安全的传法。
- **已有用户写好的 `.env.tpl`**：直接 `op run --env-file=.env.tpl -- <命令>`。本 skill 自己
  不建、不提交模板。

## 安全底线

- 发现字段只读元数据，不 `--reveal`、不为查看而 `op read`。
- 密钥不打印、不回显、不 `set -x` / 不 dump env；靠 `op run` 默认掩码兜底。
- 不在磁盘留明文。

## 错误处理

- 未登录 → 引导解锁桌面 App。
- vault / item 找不到或有歧义 → 列候选问用户。
- 字段缺失（Login item 没有 `credential`）→ 只注入存在的，并说明。

## 不做

存储 / 创建 / 轮换 / 编辑密钥；创建或提交 `.env.tpl`；service account / CI / 非交互鉴权。
