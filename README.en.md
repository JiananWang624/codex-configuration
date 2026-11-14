# Codex Configuration for CS and Robotics Research

English | [简体中文](readme.md)

A personal Codex setup developed by a PhD student in computer science and robotics for reproducing papers, running experiments, debugging research code, and building small tools. It prioritizes correctness, reproducibility, and readable code, while aiming to reduce usage through a division of work between GPT-6.1 Sol, GPT-6.1 Terra, and GPT-6 Luna.

## Files

- [config.toml](config.toml): Default model, reasoning effort, subagent concurrency, and client preferences.
- [AGENTS.md](AGENTS.md): Rules for minimal changes, research correctness, targeted retrieval, documentation and handoff, validation, and delegation.
- [agents/](agents/): Model settings and responsibilities for each custom agent. Permissions are inherited from the parent session.

## Models and responsibilities

| Role | Model / reasoning effort | Responsibility |
| --- | --- | --- |
| Primary agent | GPT-6.1 Sol / medium; high in Plan mode | Planning, architectural and scientific decisions, resolving ambiguity, delegation, and final acceptance |
| `scout` | GPT-6 Luna / high | Inspect code, configuration, documentation, and logs without making changes; return relevant evidence |
| `quick_implementer` | GPT-6 Luna / high | Make clearly scoped local changes using an already decided approach and run focused checks |
| `system_implementer` | GPT-6.1 Terra / high | Carry out substantial, already decided work under a clear implementation contract, including module interactions, integration paths, and validation; escalate conflicts with repository evidence |
| `debugger` | GPT-6.1 Sol / high | Reproduce non-obvious failures and establish the root cause; edit application code only when explicitly assigned a fix |
| `reviewer` | GPT-6.1 Sol / high | Review specific correctness, regression, or research risks without modifying files |

Subagents without a custom role default to GPT-6.1 Sol / medium. Manual `/review` uses GPT-6.1 Sol. Multi-Agent V2 is explicitly enabled, with a limit of two concurrent subagent threads, excluding the primary agent. Luna agents do not make architectural, algorithmic, or experimental-method decisions. If a task exceeds their scope, they stop and return it to the primary agent, which decides whether to handle it directly or delegate it to `system_implementer`.

Terra / high is used only after the primary agent has defined the approach, constraints, and acceptance criteria. OpenAI's [model selection guide](https://developers.openai.com/api/docs/guides/model-selection) recommends Terra for scoped code fixes and suggests comparing it with Sol using the same inputs and reasoning effort. This does not establish that Terra is suitable for every large integration task. The current assignment is a starting point, not a benchmarked result; the primary agent can handle complex integrations with Sol. Future adjustments should reflect observed quality, rework, and usage.

## Design principles

- **Keep code simple and readable.** Reuse existing implementations and dependencies, make local changes, and respect existing module boundaries. Split code when responsibilities or actual reuse justify it. Avoid compressing code just to reduce line count or adding files and layers for their own sake.
- **Preserve research semantics.** Retain reference algorithms, numerical behavior, and relevant dependency versions. Do not silently change coordinate conventions, units, normalization, dataset splits, metrics, seeds, or experiment protocols. Resolve material ambiguity through evidence or clarification.
- **Delegate when it pays off.** Use subagents when context isolation, specialization, or independent work justifies the extra cost. Give Luna clearly scoped tasks and `system_implementer` substantial, already approved implementations. Keep work coupled to unresolved research or architectural decisions in the primary agent. Normally use one subagent; use a second only for independent work and avoid overlapping edits.
- **Keep context focused and reuse agents.** Delegated tasks include the objective, decided constraints, relevant files, acceptance criteria, and validation requirements. When the tool supports `fork_turns`, set it explicitly: default to `"none"` and use `"all"` only when necessary. Use limited-history options only if supported. Reuse an existing agent for related follow-up work. Subagents cannot spawn further agents without authorization from the primary agent.
- **Match validation to the change.** Run the smallest sufficient tests, builds, or smoke checks rather than the full suite by default. Add review only for a specific unresolved risk. When numerical behavior changes, compare against a reference or check invariants using project-defined tolerances; successful execution alone does not establish scientific correctness.
- **Control experiment costs.** Small, focused tests can run automatically. Expensive training, large simulations, multi-seed experiments, parameter sweeps, large downloads, and real-hardware actions require explicit approval. Agree on resource budgets at the project or task level, proceed within the authorized scope and budget, and ask before expanding either.

## Experiment records and handoff

Global guidance defines documentation and on-demand reading principles; each project supplies its paths, commands, experiment protocols, and budgets. Use project guidance, READMEs, or existing indexes to locate relevant sections, batch independent lookups within that scope, and expand as evidence requires.

Reuse existing records, with searchable entries linking to authoritative procedures and evidence. Keep current-state summaries short, state applicability, validation status, and supersession, and link experimental results to the code, configuration, and protocol versions used. Add indexes or split documents only when navigation requires it; no fixed template or routine reports are required. The authoritative rules are in [Documentation and handoff](AGENTS.md#documentation-and-handoff).

## Installation and verification

This repository is a configuration template. Opening it does not automatically activate the root configuration as project settings.

For personal use, merge or copy [config.toml](config.toml), [AGENTS.md](AGENTS.md), and [agents/](agents/) into your Codex home directory, preserving their relative paths and any existing personal settings. The default directory is `~/.codex/`, unless `CODEX_HOME` is set. Keep the role files in the `agents/` subdirectory.

For project-only use, place the configuration and role files under the trusted project's `.codex/` directory, retaining the same structure. Place the project instructions in the project root. Project-specific commands and resource budgets belong in project guidance. See OpenAI's [configuration guide](https://learn.chatgpt.com/docs/config-file/config-basic).

Start a new session after making changes. Check the selected model, reasoning effort, available roles, and effective permissions, and ask the agent to summarize the instructions it loaded. In the CLI, use `/status` and `/debug-config`; in the IDE or desktop app, check the current session settings. Command-line or host overrides can change defaults. A file that parses successfully is not proof that every setting is active in every client.

Subagents inherit the parent session's permissions. A `sandbox_mode` entry in a custom agent file does not create an independent sandbox. The read-only behavior of `scout` and `reviewer` is an instruction, not a separate permission boundary. For enforced read-only access, select the appropriate permissions on the parent session. For everyday development, `workspace-write` with `on-request` allows permitted operations within the workspace without approval for every action. See the [subagent permissions guide](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls).

## Local preferences and portability

The shared [configuration](config.toml) omits personal MCP servers, absolute local paths, and plugin or marketplace settings tied to a specific installation. Configure MCP servers and plugins locally. Desktop appearance settings are client preferences.

The `[windows]` section is Windows-specific and can be omitted on other systems. This setup retains `elevated`. Before switching to MXC, verify compatibility, especially the lifecycle of background experiment processes. See the [Windows sandbox guide](https://learn.chatgpt.com/docs/windows/windows-sandbox).
