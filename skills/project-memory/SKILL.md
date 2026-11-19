---
name: project-memory
description: Establish, recover, or reorganize durable records when the task requires it in a clearly multi-session project. Skip small fixes, one-off scripts, temporary analyses, and routine lookups.
---

# Project Memory

Start with project instructions, the README, or an existing index. Identify the record that owns the information needed for the task, then follow relevant topics and evidence links as needed. For a read-only recovery or review, report findings, sources, and gaps.

## Maintain the owning records

When maintenance is in scope, reuse existing paths and conventions. Add a record only for a concrete gap with information that could change a future session's reasoning or action. Keep project knowledge in its owning record and use project `AGENTS.md` to point to the relevant entry points and update rule.

When setting up durable records for a new long-running research project with no established location for them, use `docs/RESEARCH_STATE.md` for needed current-state notes. Add `docs/PROJECT_FACTS.md` for durable facts or `docs/EXPERIMENTS.md` for material experiment results when there is content to record.

- **Current state** captures the objective, approach, established findings, open questions, and next action as applicable. Rewrite or merge superseded guidance and link to relevant decision or experiment history.
- **Project facts** capture stable, reusable facts with sources and applicability. When records disagree, check code, data, or verified results for the relevant version and conditions. Correct the owning record when the evidence settles the conflict. Label inferences and unresolved claims explicitly.
- **Experiments** record a stable ID, the question, material outcome and conclusion, interpretation limits, and links to evidence. Include informative negative and null results. Update the current state or a fact only when the result changes it, and link back to the experiment record.

For important runs, record the actual code revision and whether the worktree was clean or dirty. Link the saved diff or snapshot and relevant untracked inputs when available. Link the config source and the effective settings or overrides, plus the protocol, output, and environment or lockfile when material. Identify missing provenance explicitly.

## Preserve decisions and recurring pitfalls

Keep a few live decisions in the current-state record. For decisions likely to be revisited, retain the rationale, key alternatives, supporting evidence, and what superseded them. Give decision history its own record only when the existing location makes it hard to retrieve.

For recurring errors, fix the root cause when in scope and use a focused test, lint rule, or assertion where it can prevent recurrence. Record only remaining non-obvious, costly patterns likely to recur. Capture the affected context, symptom, supported cause or uncertainty, preferred approach, what to avoid, and evidence. Use an existing record unless the number or retrieval cost of these patterns justifies a dedicated one.

Use searchable headings and link to artifacts in place. Split crowded records by topic or workstream only when retrieval repeatedly crosses unrelated material, with a short index pointing to those records. Verify the entry points and evidence links changed by the task.
