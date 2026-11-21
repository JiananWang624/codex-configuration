# 面向 CS / Robotics 研究的 Codex 配置

[English](README.en.md) | 简体中文

本仓库集中维护个人 Codex 配置、子代理和技能，用于论文复现、实验代码开发与调试，以及小工具开发。所有改动先在这里编辑和验证，验收后再由维护者手动同步到电脑和服务器。

目标是在保证代码正确性与研究可复现性的前提下，让项目结构清晰、代码与文档简洁、资料便于 agent 检索和按需读取，并通过模型分工与上下文控制减少重复工作、无关 token 和额度消耗。

## 文件说明

- [config.toml](config.toml)：主模型、推理强度、子代理并发及客户端偏好。
- [global/AGENTS.md](global/AGENTS.md) 是全局规则模板。[根目录 AGENTS.md](AGENTS.md) 指导本仓库的维护。
- [agents/](agents/)：各子代理的模型与职责边界；权限继承自主会话。
- [skills/](skills/)：个人技能目录，包含各技能的 `SKILL.md` 及附带的脚本、参考资料、模板和调用配置。

公开同步的技能只有 [systematic-debugging](skills/systematic-debugging/SKILL.md)、[project-hygiene](skills/project-hygiene/SKILL.md)、[project-memory](skills/project-memory/SKILL.md) 和 [create-slurm](skills/create-slurm/SKILL.md)。以 `$create-slurm` 显式调用 CREATE 技能。其他个人技能留在本机。[历史第三方声明](skills/THIRD_PARTY_NOTICES.md) 保留用于署名。

## 模型与任务分工

仓库模板以 GPT-6.1 Sol 作为 CLI 与 IDE 共用的默认值。客户端支持 GPT-6.2 Sol 时，可从模型选择器切换，或运行 `codex -m gpt-6.2-sol`。 [Codex 模型指南](https://learn.chatgpt.com/docs/models#configure-your-default-local-model)

| 角色 | 模型 / 推理强度 | 职责 |
| --- | --- | --- |
| 主代理 | GPT-6.1 Sol / medium，Plan 模式 high | 规划、架构与科学决策、处理歧义、委派和最终验收 |
| `scout` | GPT-6 Luna / high | 只读检索代码、配置、文档和日志，返回关键证据 |
| `quick_implementer` | GPT-6 Luna / high | 已决定、范围明确的局部修改及针对性验证 |
| `system_implementer` | GPT-6.1 Terra / high | 按明确契约完成较大实现与集成；发现证据与契约冲突时交回主代理 |
| `debugger` | GPT-6.1 Sol / high | 复现非显然故障并定位根因；仅在明确委派修复时改代码 |
| `reviewer` | GPT-6.1 Sol / high | 只读审查具体的正确性、回归或研究语义风险 |

未指定模型和推理强度的子代理继承主会话设置，`/review` 使用当前会话模型。命名角色使用表中的设置。Multi-Agent V2 最多同时打开两个子代理线程，不含主代理，通常只使用一个。 [子代理配置指南](https://learn.chatgpt.com/docs/agent-configuration/subagents#choosing-models-and-reasoning)

Terra 承担方案、约束和验收标准已明确的实现；复杂集成可由主代理使用 Sol 完成。这是工作流默认分工，适用性与节省效果尚未经过同任务对比验证，后续按质量、返工和用量调整。[官方模型选择指南](https://developers.openai.com/api/docs/guides/model-selection)

## 科研工作流与项目记录

跨项目的科研、资源和验证规则放在全局模板里，各项目用自己的 `AGENTS.md` 指向已有记录。 [全局规则模板](global/AGENTS.md)

| 痛点 | 解决方式 | 对应文件 |
| --- | --- | --- |
| 换会话丢背景，旧结论与重复事实越积越多 | 复用现有状态、事实和实验记录。每条知识有一个主位置，按对应版本的代码、数据或已验证结果更新。 | [`$project-memory`](skills/project-memory/SKILL.md#maintain-the-owning-records) |
| 阴性结果容易丢，旧实验难复跑 | 用稳定的实验 ID 记录结论、代码版本和工作树状态、配置来源与生效参数，并链接原始证据。保留有价值的失败和无差异结果。 | [`$project-memory`](skills/project-memory/SKILL.md#maintain-the-owning-records) |
| 忘记为何选方案，或什么证据推翻了旧决策 | 在现有记录里保存关键备选、理由和证据。当前状态只呈现仍有效的决定，并链接历史。 | [`$project-memory`](skills/project-memory/SKILL.md#preserve-decisions-and-recurring-pitfalls) |
| Agent 跨会话反复踩同一个坑 | 优先修根因或加自动检查。只记录仍可能重现、代价高且不易察觉的问题。 | [`$project-memory`](skills/project-memory/SKILL.md#preserve-decisions-and-recurring-pitfalls) |
| 旧代码、依赖、配置和产物堆积，清理可能误删实验依据 | 清理前查实际调用和实验依赖。按授权范围处置，保留仍支撑溯源或用途不明的内容。 | [`$project-hygiene`](skills/project-hygiene/SKILL.md#repository-assets) |
| 路径或工具变了，旧指令失效或冲突 | 审计实际加载的规则、命令和路径。核实旧变通方案是否仍有用，再修改主规则。 | [`$project-hygiene`](skills/project-hygiene/SKILL.md#codex-instructions) |
| 每次读完整项目记录，占用上下文 | 从项目入口定位相关索引或章节，按需追到原始产物。 | [全局查阅规则](global/AGENTS.md#core-principles) |
| 记忆机制本身变成负担 | 沿用已有 Markdown 和 Git 记录。只在确实缺记录时新增，反复难找时再按主题拆分。 | [`$project-memory`](skills/project-memory/SKILL.md) |
| 琐事和临时猜测挤占长期记录 | 只更新会改变未来判断或行动的信息，区分已观察事实、推断和未验证说法。 | [全局记录规则](global/AGENTS.md#documentation-and-handoff) |

长期项目确实需要建立、恢复或整理记录时，[`project-memory`](skills/project-memory/SKILL.md) 可自动触发。原因不明的故障可触发 [`systematic-debugging`](skills/systematic-debugging/SKILL.md)。清理通过显式调用 `$project-hygiene` 启动。

## 手动同步与加载检查

从仓库根目录启动时，Codex 会合并本机全局指令和本仓库的维护规则。`global/AGENTS.md`、根目录的 `config.toml` 及 `skills/` 是供手动部署的源文件。

1. **个人全局使用。** 验收后，将 `global/AGENTS.md` 合并到 Codex 用户目录当前生效的全局指令文件，通常是 `AGENTS.md`。用户目录默认为 `~/.codex/`，可由 `CODEX_HOME` 指定。按需合并 `config.toml` 和 `agents/`，保留机器专用设置。将选定技能的完整目录复制到 `~/.agents/skills/` 或该机器已验证的技能目录，例如本机的 `~/.codex/skills/`。每个技能保留一份。 [技能加载位置](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)
2. **仅项目使用。** 在受信任项目中，将配置放入 `.codex/config.toml`，角色放入 `.codex/agents/`，指令放在项目根目录，技能放入 `.agents/skills/`。 [官方配置指南](https://learn.chatgpt.com/docs/config-file/config-basic)
3. **检查生效情况。** 记录部署所用的仓库提交（`git rev-parse HEAD`），再从目标客户端启动新会话，核对模型、推理强度、角色、权限、指令和技能。CLI 可用 `/status`、`/debug-config` 和 `/skills`。IDE、桌面端查看会话设置，命令行或宿主可能覆盖默认值。

**从旧版本迁移时**

- 确认目标客户端、版本和生效的 `CODEX_HOME`。其中非空的 `AGENTS.override.md` 优先于 `AGENTS.md`。将模板规则合并到实际生效的文件，去除重复条目，保留机器专用路径、MCP 设置和凭据。 [全局指令加载规则](https://learn.chatgpt.com/docs/agent-configuration/agents-md#how-codex-discovers-guidance)
- 在客户端实际加载的技能目录中检查同名副本和旧版 `research-reproduction`。由维护者决定这些副本的去留，保留其他来源的技能。

从旧模板迁移时，若希望 `/review` 和普通子代理继承会话设置，请移除已有的 `review_model`、`agents.default_subagent_model` 与 `agents.default_subagent_reasoning_effort` 覆盖项。

子代理继承主会话权限，角色文件的 `sandbox_mode` 不会建立独立沙箱；`scout` 与 `reviewer` 的只读要求属于行为约束。强制只读需在主会话设置；日常开发可选择 `workspace-write` 与 `on-request`。[官方权限说明](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls)

MCP、插件及本机路径在本地单独配置，桌面外观属于客户端偏好。共享模板通过 `features.prefer_mxc = true` 优先选择可用且策略允许的 MXC，并保留 `windows.sandbox = "elevated"` 作为其他 Windows 主机的选择回退；这不是命令失败后的自动重试，也不改变 Linux 的沙箱实现。[官方 Windows 沙箱指南](https://learn.chatgpt.com/docs/windows/windows-sandbox)

部署前确认目标客户端支持这些设置，并实际执行一个低成本只读文件测试；解析、登录或 doctor 正常不等于命令可执行。保留机器专用覆盖：本机已验证的 `windows.sandbox = "mxc"` 可以继续使用，但不能假设显式 MXC 会回退。旧客户端先核对版本支持，不要忽略未知设置告警。切换前备份配置，并检查后台进程及 SSH/tmux/Slurm 工作流兼容性。不要通过放宽权限、重置防火墙或删除共享沙箱状态来让测试通过。
