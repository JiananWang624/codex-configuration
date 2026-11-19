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

The template uses GPT-6.1 Sol as a shared CLI and IDE default. On clients that offer GPT-6.2 Sol, select it in the model picker or run `codex -m gpt-6.2-sol`. [Codex models](https://learn.chatgpt.com/docs/models#configure-your-default-local-model)

| Role | Model / reasoning effort | Responsibility |
| --- | --- | --- |
| Primary agent | GPT-6.1 Sol / medium (high in Plan mode) | Planning, architectural and scientific decisions, resolving ambiguity, delegation, and final acceptance |
| `scout` | GPT-6 Luna / high | Read-only inspection of code, configuration, documentation, and logs; return relevant evidence |
| `quick_implementer` | GPT-6 Luna / high | Clearly scoped, already decided local changes and focused validation |
| `system_implementer` | GPT-6.1 Terra / high | Substantial implementation and integration under a clear contract; escalate conflicts with repository evidence |
| `debugger` | GPT-6.1 Sol / high | Reproduce non-obvious failures and establish root cause; edit code only when explicitly assigned a fix |
| `reviewer` | GPT-6.1 Sol / high | Read-only review of specific correctness, regression, or research risks |

Subagents inherit the session model and reasoning effort when neither is configured. `/review` uses the current session model. Named roles use the settings in the table. Multi-Agent V2 permits up to two open subagent threads, excluding the primary. Normally use one. [Subagent configuration](https://learn.chatgpt.com/docs/agent-configuration/subagents#choosing-models-and-reasoning)

Terra handles implementation after the approach, constraints, and acceptance criteria are defined; the primary agent can use Sol for complex integrations. This is a workflow default, not a same-task benchmark of suitability or savings. Adjust it based on quality, rework, and usage. See the [model selection guide](https://developers.openai.com/api/docs/guides/model-selection).

## Research workflows and project records

The global template holds research, resource, and validation rules that apply across projects. Each project's `AGENTS.md` points to existing records. [Global instruction template](global/AGENTS.md)

| Problem | How the configuration addresses it | Where it is defined |
| --- | --- | --- |
| Context disappears across sessions and old or duplicate facts accumulate | Reuse current state, facts, and experiment records. Give each fact one authoritative home and update it from the applicable code, data, or verified results. | [`$project-memory`](skills/project-memory/SKILL.md#maintain-the-owning-records) |
| Negative results get lost and old runs are hard to reproduce | Use stable experiment IDs. Record the conclusion, code revision, worktree state, config source, and effective settings. Link the original evidence and retain informative failures and null results. | [`$project-memory`](skills/project-memory/SKILL.md#maintain-the-owning-records) |
| Decision rationale and the evidence that overturned it get lost | Record key alternatives, rationale, and evidence in existing records. Keep current decisions in the project state and link to their history. | [`$project-memory`](skills/project-memory/SKILL.md#preserve-decisions-and-recurring-pitfalls) |
| Agents repeat the same mistakes across sessions | Fix the cause or add an automated check where possible. Record remaining costly, non-obvious pitfalls that are likely to recur. | [`$project-memory`](skills/project-memory/SKILL.md#preserve-decisions-and-recurring-pitfalls) |
| Stale assets accumulate and cleanup can erase experiment evidence | Check real usage and experiment dependencies first. Clean up within the authorized scope and retain items with unresolved usage or provenance value. | [`$project-hygiene`](skills/project-hygiene/SKILL.md#repository-assets) |
| Paths and tools change, leaving stale or conflicting instructions | Audit the rules, commands, and paths actually in use. Verify whether old workarounds still matter before updating the authoritative rule. | [`$project-hygiene`](skills/project-hygiene/SKILL.md#codex-instructions) |
| Reading every project record consumes context | Follow project guidance to the relevant index or section and open raw artifacts as needed. | [Global reading rules](global/AGENTS.md#core-principles) |
| Project memory becomes another system to maintain | Reuse Markdown and Git records. Add records for real gaps and split by topic when retrieval repeatedly becomes difficult. | [`$project-memory`](skills/project-memory/SKILL.md) |
| Routine details and tentative guesses crowd out durable knowledge | Update records only when the information can change future judgment or action. Distinguish observations, inferences, and unverified claims. | [Global record rules](global/AGENTS.md#documentation-and-handoff) |

[`project-memory`](skills/project-memory/SKILL.md) can activate automatically when the current task in a multi-session project requires establishing, recovering, or reorganizing durable records. Failures with an unclear cause can activate [`systematic-debugging`](skills/systematic-debugging/SKILL.md). Invoke `$project-hygiene` explicitly for cleanup.

## Manual sync and verification

When launched from this repository's root, Codex combines the machine's global instructions with the repository maintenance guidance. The `global/AGENTS.md` template, root `config.toml`, and `skills/` are source files for manual deployment.

1. **Personal global use.** After approval, merge `global/AGENTS.md` into the active global instructions in the Codex home, usually `AGENTS.md`. The default home is `~/.codex/` or the location set by `CODEX_HOME`. Merge selected `config.toml` and `agents/` settings as needed, keeping machine-specific settings. Copy each selected skill directory in full into `~/.agents/skills/` or a verified location on the target machine, such as `~/.codex/skills/` on this workstation. Keep one copy per skill. [Skill discovery paths](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)
2. **Project-only use.** In a trusted project, put configuration in `.codex/config.toml`, roles in `.codex/agents/`, project instructions at the root, and skills in `.agents/skills/`. [Configuration guide](https://learn.chatgpt.com/docs/config-file/config-basic)
3. **Verify.** Record the deployed repository commit (`git rev-parse HEAD`), then start a new session in the target client and check the model, reasoning effort, roles, permissions, instructions, and skills. Use `/status`, `/debug-config`, and `/skills` in the CLI. Check session settings in the IDE or desktop app. Command-line or host overrides can change defaults.

**When migrating from an older version**

- Confirm the target client, version, and effective `CODEX_HOME`. A non-empty `AGENTS.override.md` there takes precedence over `AGENTS.md`. Merge the template into the active file, remove duplicate rules, and preserve machine-specific paths, MCP settings, and credentials. [Global instruction discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md#how-codex-discovers-guidance)
- Check the client's active skill directories for same-name copies and the former `research-reproduction` skill. The maintainer decides which of these copies to retain. Preserve skills from other sources.

When merging an older template and adopting session inheritance, remove existing `review_model`, `agents.default_subagent_model`, and `agents.default_subagent_reasoning_effort` overrides.

Subagents inherit parent permissions; `sandbox_mode` in a role file does not create a separate sandbox. Read-only requirements for `scout` and `reviewer` are behavioral instructions. Enforce read-only access on the parent session; for everyday development, you can select `workspace-write` with `on-request`. See the [permissions guide](https://learn.chatgpt.com/docs/agent-configuration/subagents#approvals-and-sandbox-controls).

## Local preferences and portability

Configure MCP servers, plugins, and machine-specific paths locally; desktop appearance settings are client preferences. `[windows]` is Windows-specific and can be omitted elsewhere. This setup uses `elevated`; before changing sandbox implementations, check compatibility with local background processes and SSH/tmux/Slurm experiment workflows. See the [Windows sandbox guide](https://learn.chatgpt.com/docs/windows/windows-sandbox).
