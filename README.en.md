# Codex Configuration for CS and Robotics Research

English | [简体中文](readme.md)

This repository maintains personal Codex configuration, subagents, and skills for paper reproduction, experiment development and debugging, and small tools. Edit and validate changes here, then manually sync approved changes to workstations and servers.

The goal is correct, reproducible research with clear project structure, concise code and documentation, and information agents can find and read on demand. Model specialization and focused context aim to reduce repeated work, irrelevant tokens, and usage.

## Files

- [config.toml](config.toml): Default model, reasoning effort, subagent concurrency, and client preferences.
- [global/AGENTS.md](global/AGENTS.md) is the global instruction template. The [root AGENTS.md](AGENTS.md) guides maintenance of this repository.
- [agents/](agents/): Each subagent's model and responsibilities; permissions are inherited from the parent session.
- [skills/](skills/): Personal skills, with each skill's `SKILL.md` and any bundled scripts, references, templates, and invocation settings.

See [third-party notices](skills/THIRD_PARTY_NOTICES.md) for the source and license of the four adapted scientific skills.

## Models and responsibilities

| Role | Model / reasoning effort | Responsibility |
| --- | --- | --- |
| Primary agent | GPT-6.1 Sol / medium; high in Plan mode | Planning, architectural and scientific decisions, resolving ambiguity, delegation, and final acceptance |
| `scout` | GPT-6 Luna / high | Read-only inspection of code, configuration, documentation, and logs; return relevant evidence |
| `quick_implementer` | GPT-6 Luna / high | Clearly scoped, already decided local changes and focused validation |
| `system_implementer` | GPT-6.1 Terra / high | Substantial implementation and integration under a clear contract; escalate conflicts with repository evidence |
| `debugger` | GPT-6.1 Sol / high | Reproduce non-obvious failures and establish root cause; edit code only when explicitly assigned a fix |
| `reviewer` | GPT-6.1 Sol / high | Read-only review of specific correctness, regression, or research risks |

Subagents without a custom role default to Sol / medium; manual `/review` uses Sol. Multi-Agent V2 is enabled, with at most two concurrently open spawned-agent threads, excluding the primary; normally use one. Luna agents return architectural, algorithmic, or experimental-method decisions to the primary agent.

Terra handles implementation after the approach, constraints, and acceptance criteria are defined; the primary agent can use Sol for complex integrations. This is a workflow default, not a same-task benchmark of suitability or savings. Adjust it based on quality, rework, and usage. See the [model selection guide](https://developers.openai.com/api/docs/guides/model-selection).

## Research workflows and project records

The global template defines [research and resource boundaries](global/AGENTS.md#research-correctness-and-resource-safety) and [experiment records and handoff](global/AGENTS.md#documentation-and-handoff). Each project's `AGENTS.md` points to its existing state, facts, and experiment records. When implementation produces a result that changes future judgment or action, update the owning record and link the evidence. Use the project overview to record the result's effect on current direction.

- Explicitly invoke [`$project-memory`](skills/project-memory/SKILL.md) to establish, recover, or maintain durable project records.
- Explicitly invoke [`$project-hygiene`](skills/project-hygiene/SKILL.md) to audit stale repository assets or Codex instructions and carry out authorized cleanup.

Among personal skills, only [`systematic-debugging`](skills/systematic-debugging/SKILL.md) can activate automatically, for failures or anomalies with an unclear cause.

## Manual sync and verification

When launched from this repository's root, Codex combines the machine's global instructions with the repository maintenance guidance. The `global/AGENTS.md` template, root `config.toml`, and `skills/` are source files for manual deployment.

1. **Personal global use.** After approval, merge `global/AGENTS.md` into `AGENTS.md` in each machine's Codex home. The default home is `~/.codex/` or the location set by `CODEX_HOME`. Merge selected `config.toml` and `agents/` settings as needed, keeping machine-specific settings. Copy each selected skill directory in full into one verified skill location on the target machine. This workstation currently uses `~/.codex/skills/`. Each client manages its own bundled skills and plugins.
2. **Project-only use:** Place configuration and role files under the trusted project's `.codex/` directory, and project instructions at the project root. See the [configuration guide](https://learn.chatgpt.com/docs/config-file/config-basic) for loading precedence.
3. **Verify:** Start a new session and check the model, reasoning effort, roles, permissions, instructions, and skills. Use `/status`, `/debug-config`, and `/skills` in the CLI. Check session settings in the IDE or desktop app. Command-line or host overrides can change defaults.

Subagents inherit parent permissions; `sandbox_mode` in a role file does not create a separate sandbox. Read-only requirements for `scout` and `reviewer` are behavioral instructions. Enforce read-only access on the parent session; for everyday development, you can select `workspace-write` with `on-request`. See the [permissions guide](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls).

## Local preferences and portability

Configure MCP servers, plugins, and machine-specific paths locally; desktop appearance settings are client preferences. `[windows]` is Windows-specific and can be omitted elsewhere. This setup uses `elevated`; before changing sandbox implementations, check compatibility with local background processes and SSH/tmux/Slurm experiment workflows. See the [Windows sandbox guide](https://learn.chatgpt.com/docs/windows/windows-sandbox).
