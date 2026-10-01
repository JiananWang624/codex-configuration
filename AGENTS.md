# Global AGENTS.md

## Core principles

Prefer the smallest correct implementation that fully solves the requested task.

Start with targeted searches. Read only the code, configuration, tests, documentation, and dependencies needed for the task.

Reuse existing code and dependencies before adding new code, files, abstractions, configuration, or dependencies.

For non-trivial functionality not already available, prefer a suitable maintained upstream package or mature open-source implementation over writing a custom replacement. Do not add dependencies for trivial functionality.

When reproducing or comparing against a paper, preserve the reference implementation, algorithm, numerical behavior, or dependency version whenever those details could affect reproducibility.

## Efficiency

Use effort proportionate to the change.

Escalate testing, review, investigation, or subagent use only for a concrete unresolved risk.

## Implementation

Keep changes local and minimal. Do not perform unrelated refactoring, renaming, formatting, cleanup, or architecture changes.

Avoid speculative abstractions and defensive complexity. Do not add wrappers, managers, factories, adapters, compatibility layers, feature flags, retries, fallback paths, or extra state unless required by the task or an evidenced failure mode.

Prefer simple direct code over abstractions used only once. Add comments only when they explain why something is necessary.

Keep one authoritative implementation path for the same production behavior. Parallel implementations are allowed when intentionally retained as research baselines, ablations, paper variants, comparison methods, or reproducibility references.

When the requested change intentionally replaces production behavior, validate the replacement and remove only obsolete code, wiring, configuration, dependencies, or consumers made unnecessary by that replacement. Keep unrelated cleanup out of scope. Do not remove intentionally retained research variants.

If the selected implementation cannot operate correctly, fail clearly with actionable diagnostics rather than silently falling back.

## Working style

For explanation, review, diagnosis, or planning requests, inspect and report without modifying files.

For implementation, fix, or build requests, make the in-scope change and perform the applicable non-destructive validation without asking first.

Ask only when a missing decision materially changes the result or when an action is destructive, external, privileged, costly, or otherwise requires approval.

Use Notion only when the user's prompt explicitly asks for it.

Preserve existing user changes. Do not use destructive Git commands, force-push, broad restore operations, or overwrite unrelated work.

## Research correctness

For research, robotics, simulation, or machine-learning code, do not silently change mathematical definitions, algorithm behavior, coordinate conventions, units, timing, normalization, dataset splits, evaluation metrics, random seeds, or experiment protocols.

If such a choice is ambiguous or could materially affect experimental results, do not guess. Subagents must escalate it to the primary agent; the primary agent should resolve it from evidence or ask the user when a genuinely missing decision materially affects the result.

Do not launch expensive training, large simulations, multi-seed experiments, hyperparameter sweeps, large downloads, or real-hardware actions without explicit approval. Small targeted tests and smoke tests may run automatically.

## Validation

Run the smallest sufficient validation for the affected behavior. Prefer focused tests, targeted builds, static checks, and smoke tests over broad project-wide validation.

Do not run the full test suite by default. Use broader validation only when the change has broad impact, narrower checks are insufficient, targeted validation reveals wider risk, or the task explicitly reaches a final integration/release regression stage.

Do not repeat tests or benchmarks that already produced valid inspected evidence unless relevant code changed or a concrete coverage gap was identified.

After validation, inspect the final diff once. Once the acceptance criteria are satisfied, proceed or finish; do not add extra validation or review solely for additional confidence.

## Long-running commands

For long-running non-interactive commands, wait on the existing process using the longest practical interval when intermediate output is unnecessary. Poll more frequently only when intermediate output or interaction is useful.

## Communication

Be concise. Report what changed, validation performed, and anything still unverified. Avoid long progress narration unless it helps a decision or the user asks for it.

## Agent usage
When the conditions below are met, this AGENTS.md explicitly authorizes and instructs the primary agent to delegate work to the named subagent without asking the user for additional confirmation.

The primary agent owns planning, architecture, scientific and algorithmic decisions, ambiguity resolution, orchestration, and final acceptance.

Delegate only when context isolation, specialization, or genuinely
independent work provides enough benefit to justify the additional agent
and token cost.

When delegating bounded work, give the subagent a self-contained task that
includes the objective, already-decided constraints, relevant files or
symbols when known, acceptance criteria, and required validation.

Prefer minimal parent-history inheritance. Use `fork_turns="none"` for
self-contained tasks by default, a small positive number when recent parent
turns materially help, and `"all"` only when the complete parent history is
genuinely necessary.

Subagents must not spawn further subagents unless the primary agent
explicitly delegates that authority.

Give `scout` and `quick_implementer` only clearly bounded tasks that do not require scientific or architectural decisions.

During Plan mode, subagents may investigate, but do not begin source implementation until planning is complete and execution starts.

Use `scout` for read-heavy repository investigation, code mapping, documentation lookup, configuration inspection, or log analysis when this would keep substantial noise out of the primary context.

For implementation:

* Use `quick_implementer` for clear, local, already-decided changes that follow an existing pattern and have objective validation.
* Use `system_implementer` for substantial, already-approved implementation that can be expressed as a self-contained implementation contract and where isolating implementation context from the primary agent is beneficial. This commonly includes interacting modules, integration paths, or substantial implementation and validation work.
* Keep implementation in the primary agent when it remains tightly coupled to unresolved architectural, algorithmic, experimental, or scientifically meaningful decisions.

For trivial edits, the primary agent may implement directly.

If `quick_implementer` discovers that the task is broader than expected,
stop and escalate to the primary agent. The primary agent decides whether
to handle the work directly or delegate it to `system_implementer`.

For closely related follow-up work, reuse the existing subagent when its
context remains relevant rather than spawning a replacement. Spawn a new
agent for independent work, a different role, or when a clean context is
beneficial.

Use `reviewer` only when a specific residual risk is not adequately covered by existing validation. Do not use review as a default gate for non-trivial changes, and do not repeat already-successful tests without a concrete reason.

Use `debugger` for non-obvious failures or unexplained behavior. Reproduce the issue and isolate root cause before broad fixes.

Prefer one active subagent at a time. Use a second only for genuinely independent investigations or hypotheses. Do not run overlapping implementation agents concurrently, and use at most two subagents unless explicitly justified.

Subagents should return concise findings rather than raw logs. The primary agent synthesizes their findings and decides final acceptance.
