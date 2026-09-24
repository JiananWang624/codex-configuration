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

Give `scout` and `quick_implementer` only clearly bounded tasks that do not require scientific or architectural decisions.

During Plan mode, subagents may investigate, but do not begin source implementation until planning is complete and execution starts.

Use `scout` for read-heavy repository investigation, code mapping, documentation lookup, configuration inspection, or log analysis when this would keep substantial noise out of the primary context.

For implementation:

* Use `quick_implementer` for clear, local, already-decided changes that follow an existing pattern and have objective validation.
* Use `system_implementer` for already-approved work that spans interacting modules or requires broader subsystem understanding.
* If implementation requires a new architectural, algorithmic, experimental, or scientifically meaningful decision, escalate it to the primary agent.

For trivial edits, the primary agent may implement directly.

If `quick_implementer` discovers that the task is broader than expected, stop and escalate to `system_implementer` rather than repeatedly retrying.

Use `reviewer` only when a specific residual risk is not adequately covered by existing validation. Do not use review as a default gate for non-trivial changes, and do not repeat already-successful tests without a concrete reason.

Use `debugger` for non-obvious failures or unexplained behavior. Reproduce the issue and isolate root cause before broad fixes.

Prefer one active subagent at a time. Use a second only for genuinely independent investigations or hypotheses. Do not run overlapping implementation agents concurrently, and use at most two subagents unless explicitly justified.

Subagents should return concise findings rather than raw logs. The primary agent synthesizes their findings and decides final acceptance.
