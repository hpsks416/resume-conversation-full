> ⚠️ **本仓库已废弃**：内容已并入 [agent-deploy](https://github.com/hpsks416/agent-deploy) 的 skills/resume-conversation-full/ 子目录，请以 agent-deploy 为准。本仓库保留仅供历史归档。

# resume-conversation-full

继承既有对话的**完整细节**。用法：`/resume-conversation-full <对话名>`。只读、不搬移。

## 环境依赖

- 操作系统：Windows
- 运行时：Node.js v24（解压 .zstd）
- 第三方软件：无（仅依赖系统自带的 PowerShell / 标准库）

## 目录结构

    resume-conversation-full/
    ├── SKILL.md    技能入口与工作流
    ├── evals.yaml
    ├── references\conversation-sources.md

## 安装

    # GitHub
    git clone https://github.com/hpsks416/resume-conversation-full.git "$env:USERPROFILE\.dsh\skills\resume-conversation-full"
    # 或 Gitee（国内直连）
    git clone https://gitee.com/hpsks416/resume-conversation-full.git "$env:USERPROFILE\.dsh\skills\resume-conversation-full"

克隆后 DSH 自动重新发现，无需构建。

## License

MIT License. See [LICENSE](LICENSE).

