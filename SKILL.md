---
name: resume-conversation-full
description: 在对话中用 `/resume-conversation-full <对话名>` 继承既有迁移对话的完整细节——按对话名定位 conversations/*.md，全文直读（大文件分段 map-reduce），不搬移项目文件。Use when the user types "/resume-conversation-full <名字>" or asks to read a migrated conversation's full detail. Not for creating summaries or moving files.
---

# Resume Conversation (Full)

继承既有迁移对话的**完整细节**。用法：`/resume-conversation-full <对话名>`。只读、不搬移。

## 何时用

- 用户输入 `/resume-conversation-full 3D管线` 这类斜杠指令。
- 需要对话的**精确细节**（具体排障步骤、代码片段、中间讨论），不能只靠摘要。

## 何时不用

- 只要主干/结论 → 用 `resume-conversation-brief`。
- 要搬移/复制项目文件 → 本 skill 不做，直接拒绝。

## 定位对话（三级回退：Codex 迁移 .md → 索引映射 → DSH 原生会话）

> ⚠️ 坑：`glob` 在 Windows 绝对路径 + 反斜杠 + `**` 下会静默返回空（No files found），实际文件却存在。递归找文件改用 `pwsh` 的 `Get-ChildItem -Recurse`，或用 glob 走正斜杠 + `path` 参数。

1. **第一级：Codex 迁移 .md**——在 `G:\CodexDS\DSH\MAIN` 下用 pwsh 递归找 `conversations\*.md`，按**文件名去 .md 前缀匹配**对话名（如「3D管线」→ `3D管线.md`）。有语义名的直接命中。
2. **第二级：索引映射**——无语义名（`_migrated_codex\conversations\2026-09-13--<sessionid>.md`）先读 `_migrated_codex\conversations\index.md`，用「标题」列把对话名映射到 session-id 文件。
3. **第三级：DSH 原生会话回退**——前两级找不到时，用 `session_query`（query=对话名）查 DSH 原生会话注册表；命中后读 `~/.dsh\storages\session_projcache\sessions\<session-id>.json`（会话投影缓存，含 turn outline / token 统计 / 模型选择）。DSH 原生会话不是 .md，是 `.zstd` 日志 + projcache。
4. 三级都失败：报告「未找到对话『X』」，列出可用清单（Codex 语义名 + session_query 结果），不猜测。

## 读取（分两种内容源）

**源 A：Codex 迁移 .md**
- 文件 ≤ 约 40k 字符：直接全文读入。
- 文件 > 40k 字符：**分段读**（read offset/limit），逐段读完，不静默丢中间内容。

**源 B：DSH 原生会话（projcache JSON）**
- brief/full 都优先读 projcache 的 turn outline（已含 prompt/response 摘要）。
- 需要原始全文时，解压 `~/.dsh\sessions\--<workspace>--\<session-id>\session.v3.jsonl.zstd`——本机无 zstd 二进制、python 无 zstandard，但 **node v24 内置 zstdDecompressSync**，且日志是**多帧追加写**，须逐帧解压（扫帧边界 + 逐帧 decompress），单次解压只出第一帧。

读完后向用户输出：来源类型（.md / 原生会话）、路径、总规模、已完整读取的确认。

## 铁律

- **只读、不搬移**：绝不复制、移动、修改任何项目文件或 .md。
- **不静默丢内容**：大文件必须读完，读不完要明说「读到 X 处，还有 Y 未读」。
- **引用来源**：转述对话内容时，标注出自哪个来源（「据 3D管线.md」或「据会话 <session-id>」）。
- **定位优先三级回退**：Codex .md → 索引映射 → session_query 原生会话，不要只在 MAIN 找 .md 就放弃。
