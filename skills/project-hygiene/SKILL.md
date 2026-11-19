---
name: project-hygiene
description: Audit stale repository assets or Codex instructions and carry out authorized cleanup based on evidence.
---

# Project Hygiene

Start with the cleanup scope the user requests. For an audit, report findings and proposed dispositions. A cleanup request can authorize changes within its stated scope. Confirm destructive changes when that authorization is missing or unclear.

## Repository assets

Check actual use before removing stale code, scripts, configs, dependencies, docs, or temporary outputs, or consolidating duplicate implementations. Trace relevant imports, entry points, config references, scripts, docs, and experiment records. Account for dynamic or external entry points when evidence suggests they exist. Check relevant Git history when current references cannot explain the purpose. A lack of text matches alone is insufficient evidence that an asset is unused.

## Decide from the evidence

- **Remove** a candidate only when the evidence supports removal and the user authorized it.
- **Retain or archive** intentional baselines, ablations, and assets used by important experiments. Archive only within scope and verify that experiment records continue to resolve to the code, configs, checkpoints, and outputs they need.
- **Keep** actively used or uncertain candidates. Report unresolved dependencies and what evidence would settle them.

## Codex instructions

For a requested instruction audit after a repo, workflow, toolchain, or Codex change, or when skill behavior seems off, check the `AGENTS.md` files and skill instructions that apply in the affected context. Trace stale paths, commands, and routes. Check for conflicting rules, duplicated guidance, and workarounds whose original conditions may have changed.

Verify assumptions against current repo behavior, the relevant tool version, or authoritative documentation. Correct the rule in its authoritative location and retain applicable project constraints. Treat an upgrade as a reason to check whether a workaround still matters. Keep or flag uncertain cases until there is evidence to change them.

Make only the agreed changes and update affected references. Run the smallest check for the affected route, command, or evidence chain. Report the disposition and supporting evidence for material candidates.
