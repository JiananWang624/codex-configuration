---
name: create-slurm
description: Plan and operate authorized KCL CREATE Slurm workflows for resources, queue, status, logs, submissions, cancellations, and concrete failures.
---

# KCL CREATE and Slurm

Use only when explicitly invoked as `$create-slurm`. Read the relevant section of [CREATE operations](references/create.md); on a concrete fault, search [CREATE issues](references/create-issues.md) by exact error, stage or case ID. The package does not assume account access, site configuration, resource availability, or live job state.

Use existing authorized SSH configuration. Invocation is not permission to connect, change a remote system, submit or cancel. Keep plans and mocks local, and stop at authentication, host verification or network blocks without trying fallback accounts or credentials. Do not run training or other heavy compute on login nodes. Get explicit confirmation of the exact reviewed plan before any `sbatch`, including `--test-only`. Cancellation needs separate explicit approval for the named current job. Resolve ambiguous submission through queue and accounting; never retry blindly.

Only record a new real, reusable, verified lesson after permitted work. Update a matching stable entry instead of duplicating it; retain known conditions, evidence and applicability, and keep unproven causes unknown. This skill may be published: add only generic, de-identified lessons without user/host details, private paths, timestamps tied to individuals, logs, job IDs, credentials or internal topology. Keep private discoveries in the user's separate authorized local record, not this package. Skip read-only, no-documentation, routine, plan, mock, failed and unverified work. No automatic deletion, commit or push.
