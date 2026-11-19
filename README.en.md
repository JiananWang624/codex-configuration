# Codex Configuration for CS and Robotics Research

English | [简体中文](readme.md)

This repository maintains personal Codex configuration, subagents, and skills for paper reproduction, experiment development and debugging, and small tools. Edit and validate changes here, then manually sync approved changes to workstations and servers.

The goal is correct, reproducible research with clear project structure, concise code and documentation, and information agents can find and read on demand. Model specialization and focused context aim to reduce repeated work, irrelevant tokens, and usage.

## Files

- [config.toml](config.toml): Default model, reasoning effort, subagent concurrency, and client preferences.
- [AGENTS.md](AGENTS.md): Global baseline instructions, merged with project guidance.
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

## Design principles

- **Keep code and structure simple.** Reuse existing code and suitable mature dependencies, make necessary local changes, and organize modules by responsibility and actual reuse. Preserve intentional baselines, ablations, and paper variants.
- **Preserve research semantics.** Retain reference algorithms, numerical behavior, and relevant dependency versions. Resolve material ambiguity in coordinate conventions, units, data splits, metrics, seeds, or experiment protocols through evidence or clarification.
- **Use context and delegation selectively.** Delegate when context isolation or independent work justifies the cost. Minimize inherited history and reuse relevant agents. Keep work coupled to unresolved research or architectural decisions in the primary agent. Tool parameters and escalation rules are in [Agent usage](AGENTS.md#agent-usage).
- **Match validation and cost to the task.** Run the smallest sufficient checks; numerical changes need reference comparisons or invariant checks. Expensive training, large simulations, multi-seed experiments, sweeps, large downloads, and real-hardware actions require explicit authorization. Continue within the agreed scope and budget.

## Experiment records and handoff

Global guidance defines organization and reading principles; projects supply paths, commands, environments, experiment protocols, and budgets.

- **Clear entry points:** Use project guidance, READMEs, or existing indexes to locate material for the task.
- **Progressive reading:** Search keywords or existing issue/experiment identifiers for short entries, then follow links to relevant procedures and evidence. Batch independent lookups within that scope.
- **One authoritative source:** Maintain procedures centrally and reference original evidence. Link experimental results to the code, effective configuration, and protocol versions used, preserving necessary reproduction details.
- **Clear status and history:** Keep current status short, distinguish observations, inferences, and unverified claims, and state applicability. Mark superseded conclusions, link to current guidance, and preserve historical evidence.

Reuse existing records and layouts. Add indexes or split documents only when navigation requires it; no fixed template or routine reports are required. See [Documentation and handoff](AGENTS.md#documentation-and-handoff) for the authoritative rules.

## Manual sync and verification

Opening this repository does not automatically load its root `config.toml` or `skills/`. The root `AGENTS.md` applies as project guidance and merges with global guidance, so installing the same file globally duplicates its rules while editing this repository.

1. **Personal global use:** After approval, merge the desired `config.toml`, `AGENTS.md`, and `agents/` settings into the Codex home of each target machine, preserving machine-specific settings. The default is `~/.codex/`, or the location set by `CODEX_HOME`. Copy each selected skill directory in full into that home's `skills/`, for example `~/.codex/skills/research-reproduction/SKILL.md`. Each client manages its own bundled skills and plugins.
2. **Project-only use:** Place configuration and role files under the trusted project's `.codex/` directory, and project instructions at the project root. See the [configuration guide](https://learn.chatgpt.com/docs/config-file/config-basic) for loading precedence.
3. **Verify:** Start a new session and check the model, reasoning effort, roles, permissions, instructions, and skills. Use `/status`, `/debug-config`, and `/skills` in the CLI. Check session settings in the IDE or desktop app. Command-line or host overrides can change defaults.

Subagents inherit parent permissions; `sandbox_mode` in a role file does not create a separate sandbox. Read-only requirements for `scout` and `reviewer` are behavioral instructions. Enforce read-only access on the parent session; for everyday development, you can select `workspace-write` with `on-request`. See the [permissions guide](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls).

## Local preferences and portability

Configure MCP servers, plugins, and machine-specific paths locally; desktop appearance settings are client preferences. `[windows]` is Windows-specific and can be omitted elsewhere. This setup uses `elevated`; before changing sandbox implementations, check compatibility with local background processes and SSH/tmux/Slurm experiment workflows. See the [Windows sandbox guide](https://learn.chatgpt.com/docs/windows/windows-sandbox).
