# 面向 CS / Robotics 研究的 Codex 配置

[English](README.en.md) | 简体中文

本仓库集中维护个人 Codex 配置、子代理和技能，用于论文复现、实验代码开发与调试，以及小工具开发。所有改动先在这里编辑和验证，验收后再由维护者手动同步到电脑和服务器。

目标是在保证代码正确性与研究可复现性的前提下，让项目结构清晰、代码与文档简洁、资料便于 agent 检索和按需读取，并通过模型分工与上下文控制减少重复工作、无关 token 和额度消耗。

## 文件说明

- [config.toml](config.toml)：主模型、推理强度、子代理并发及客户端偏好。
- [global/AGENTS.md](global/AGENTS.md) 是全局规则模板。[根目录 AGENTS.md](AGENTS.md) 指导本仓库的维护。
- [agents/](agents/)：各子代理的模型与职责边界；权限继承自主会话。
- [skills/](skills/)：个人技能目录，包含各技能的 `SKILL.md` 及附带的脚本、参考资料、模板和调用配置。

四个科研技能的上游来源和许可见[第三方声明](skills/THIRD_PARTY_NOTICES.md)。

## 模型与任务分工

| 角色 | 模型 / 推理强度 | 职责 |
| --- | --- | --- |
| 主代理 | GPT-6.1 Sol / medium，Plan 模式 high | 规划、架构与科学决策、处理歧义、委派和最终验收 |
| `scout` | GPT-6 Luna / high | 只读检索代码、配置、文档和日志，返回关键证据 |
| `quick_implementer` | GPT-6 Luna / high | 已决定、范围明确的局部修改及针对性验证 |
| `system_implementer` | GPT-6.1 Terra / high | 按明确契约完成较大实现与集成；发现证据与契约冲突时交回主代理 |
| `debugger` | GPT-6.1 Sol / high | 复现非显然故障并定位根因；仅在明确委派修复时改代码 |
| `reviewer` | GPT-6.1 Sol / high | 只读审查具体的正确性、回归或研究语义风险 |

未指定角色的子代理默认 Sol / medium，手动 `/review` 使用 Sol。已启用 Multi-Agent V2，最多同时打开两个子代理线程，不含主代理；通常只使用一个。Luna 不承担架构、算法或实验方法决策，超出范围时交回主代理。

Terra 承担方案、约束和验收标准已明确的实现；复杂集成可由主代理使用 Sol 完成。这是工作流默认分工，适用性与节省效果尚未经过同任务对比验证，后续按质量、返工和用量调整。[官方模型选择指南](https://developers.openai.com/api/docs/guides/model-selection)

## 科研工作流与项目记录

全局模板规定[科研与资源边界](global/AGENTS.md#research-correctness-and-resource-safety)和[实验记录与交接要求](global/AGENTS.md#documentation-and-handoff)。各项目在自己的 `AGENTS.md` 指向现有的状态、事实和实验记录。普通实施产生足以改变后续判断或行动的结果时，更新对应记录，链接当时的证据。项目概览记录结果对当前方向的影响。

- 显式调用 [`$project-memory`](skills/project-memory/SKILL.md) 建立、恢复或维护长期项目记录。
- 显式调用 [`$project-hygiene`](skills/project-hygiene/SKILL.md) 审查过时的仓库文件或 Codex 指令，并执行已授权的清理。

个人技能中，只有 [`systematic-debugging`](skills/systematic-debugging/SKILL.md) 允许自动触发，用于原因不明的失败或异常。

## 手动同步与加载检查

从仓库根目录启动时，Codex 会合并本机全局指令和本仓库的维护规则。`global/AGENTS.md`、根目录的 `config.toml` 及 `skills/` 是供手动部署的源文件。

1. **个人全局使用。** 验收后，将 `global/AGENTS.md` 合并到各机器 Codex 用户目录的 `AGENTS.md`。该目录默认为 `~/.codex/`，也可以由 `CODEX_HOME` 指定。按需合并 `config.toml` 和 `agents/`，保留机器专用设置。将选定的技能目录完整复制到该机器已经验证的技能目录，每个技能保留一份。本机当前使用 `~/.codex/skills/`。内置技能和插件由各客户端管理。
2. **仅项目使用**：配置与角色文件放在受信任项目的 `.codex/` 下，项目指令放在项目根目录。具体加载顺序见[官方配置指南](https://learn.chatgpt.com/docs/config-file/config-basic)。
3. **检查生效情况**：启动新会话，核对模型、推理强度、角色、权限、指令和技能。CLI 可用 `/status`、`/debug-config` 和 `/skills`；IDE、桌面端以会话设置为准，命令行或宿主覆盖可能改变默认值。

子代理继承主会话权限，角色文件的 `sandbox_mode` 不会建立独立沙箱；`scout` 与 `reviewer` 的只读要求属于行为约束。强制只读需在主会话设置；日常开发可选择 `workspace-write` 与 `on-request`。[官方权限说明](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls)

MCP、插件及本机路径在本地单独配置，桌面外观属于客户端偏好。`[windows]` 为 Windows 专用，其他系统可省略；当前使用 `elevated`。切换沙箱实现前，需核对本机后台进程及 SSH/tmux/Slurm 实验工作流的兼容性。[官方 Windows 沙箱指南](https://learn.chatgpt.com/docs/windows/windows-sandbox)
