# 面向 CS / Robotics 研究的 Codex 配置

[English](README.en.md) | 简体中文

本仓库集中维护个人 Codex 配置、子代理和技能，用于论文复现、实验代码开发与调试，以及小工具开发。所有改动先在这里编辑和验证，验收后再由维护者手动同步到电脑和服务器。

目标是在保证代码正确性与研究可复现性的前提下，让项目结构清晰、代码与文档简洁、资料便于 agent 检索和按需读取，并通过模型分工与上下文控制减少重复工作、无关 token 和额度消耗。

## 文件说明

- [config.toml](config.toml)：主模型、推理强度、子代理并发及客户端偏好。
- [AGENTS.md](AGENTS.md)：全局默认规则；与项目指令分层合并。
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

## 配置思路

- **代码与结构简洁**：优先复用现有代码和合适的成熟依赖，只做必要的局部修改；按职责和实际复用组织模块，保留有意维护的基线、消融和论文变体。
- **科研语义优先**：保留参考算法、数值行为和相关依赖版本；坐标系、单位、数据划分、指标、种子和实验协议等有实质歧义时先查证或询问。
- **上下文与委派按需**：仅在隔离上下文或独立工作值得额外成本时委派，最小化历史继承并复用相关代理；科研或架构决策紧密耦合的任务留在主代理。工具参数与升级规则见 [Agent usage](AGENTS.md#agent-usage)。
- **验证与成本匹配**：执行最小充分验证，数值变化需要参考比较或不变量检查。昂贵训练、大规模仿真、多种子实验、参数扫描、大量下载及真实硬件操作需明确授权；在约定范围和预算内连续推进。

## 实验记录与交接

全局配置规定组织与读取原则；实际路径、命令、环境、实验协议和预算由项目提供。

- **入口明确**：通过项目指导、README 或已有索引，按任务定位对应资料。
- **渐进式读取**：用关键词或已有问题／实验编号定位简短条目，再沿链接展开相关步骤和证据；相关范围内的独立查找可批量进行。
- **单一权威来源**：操作步骤集中维护，记录引用原始证据；实验结果关联当时的代码、有效配置和协议版本，保留必要复现条件。
- **状态与历史清楚**：当前状态简短，区分观察、推断和待验证结论；注明适用条件及已被替代的结论，链接到当前方案并保留历史证据。

复用已有记录和布局，仅在导航不足时增加索引或拆分文档，不强制模板或例行报告。细则见 [Documentation and handoff](AGENTS.md#documentation-and-handoff)。

## 手动同步与加载检查

打开本仓库不会自动加载根目录的 `config.toml` 和 `skills/`。根目录 `AGENTS.md` 会作为项目指导与全局指导合并；如果两份文件内容相同，规则会重复。

1. **个人全局使用**：验收后，将所需的 `config.toml`、`AGENTS.md` 和 `agents/` 内容合并到各机器的 Codex 用户目录（默认 `~/.codex/`，或 `CODEX_HOME` 指定位置），保留已有的机器专用设置。把选定的技能目录完整复制到用户目录的 `skills/` 下，例如 `~/.codex/skills/research-reproduction/SKILL.md`。内置技能和插件由各客户端管理。
2. **仅项目使用**：配置与角色文件放在受信任项目的 `.codex/` 下，项目指令放在项目根目录。具体加载顺序见[官方配置指南](https://learn.chatgpt.com/docs/config-file/config-basic)。
3. **检查生效情况**：启动新会话，核对模型、推理强度、角色、权限、指令和技能。CLI 可用 `/status`、`/debug-config` 和 `/skills`；IDE、桌面端以会话设置为准，命令行或宿主覆盖可能改变默认值。

子代理继承主会话权限，角色文件的 `sandbox_mode` 不会建立独立沙箱；`scout` 与 `reviewer` 的只读要求属于行为约束。强制只读需在主会话设置；日常开发可选择 `workspace-write` 与 `on-request`。[官方权限说明](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls)

MCP、插件及本机路径在本地单独配置，桌面外观属于客户端偏好。`[windows]` 为 Windows 专用，其他系统可省略；当前使用 `elevated`。切换沙箱实现前，需核对本机后台进程及 SSH/tmux/Slurm 实验工作流的兼容性。[官方 Windows 沙箱指南](https://learn.chatgpt.com/docs/windows/windows-sandbox)
