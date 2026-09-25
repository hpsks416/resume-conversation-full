# resume-conversation-full

在对话中用 `/resume-conversation-full <对话名>` 继承既有迁移对话的完整细节——按对话名定位 conversations/*.md，全文直读（大文件分段 map-reduce），不搬移项目文件。Use when the user types "/resume-conversation-full <名字>" or asks to read a migrated conversation's full detail. Not for creating summaries or moving files.

## 这是什么

DSH（DeepSeek Harness）skill —— 一个可由 AI agent 按需自动加载的能力单元。克隆到 skill 目录后，DSH 会依据上方描述自动发现并触发它，无需构建。

## 安装

最简单：用 [dsh-config](https://github.com/hpsks416/dsh-config) 的一键脚本 `install.ps1` 批量安装全部 skill。单个安装：

    # GitHub
    git clone https://github.com/hpsks416/resume-conversation-full.git "$env:USERPROFILE\.dsh\skills\resume-conversation-full"
    # 或 Gitee（国内直连更快）
    git clone https://gitee.com/hpsks416/resume-conversation-full.git "$env:USERPROFILE\.dsh\skills\resume-conversation-full"

克隆后 DSH 会自动重新发现，无需重启。更新用：

    git -C "$env:USERPROFILE\.dsh\skills\resume-conversation-full" pull

## 目录结构

    resume-conversation-full/
    ├── SKILL.md    技能入口与工作流
    ├── evals.yaml

## 依赖

无运行时依赖，纯指令型 skill（由 agent 直接执行 Markdown 工作流）。

## License

MIT License. See [LICENSE](LICENSE).
