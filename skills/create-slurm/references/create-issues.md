# CREATE issue patterns

These are troubleshooting patterns, not current incident records or proof of access. Start with [CREATE operations](create.md) for the applicable steps.

<a id="ssh-01"></a>
## SSH-01 — connection, MFA and public-key stages

Timeout, reset or denial before authentication is transport or access-policy evidence, not a public-key rejection. A visible MFA banner also does not prove failure or success. An already pending interactive MFA request stays in the same process for the user to approve through the official interface. Stop on denial or unverified host identity; do not retry with alternative credentials, change trust settings, or infer an account issue from a port error.

<a id="shell-01"></a>
## SHELL-01 — Windows multiline quoting

If a query only outputs its heading or its arguments are unexpectedly split, inspect both shells’ argument boundaries. A reviewed UTF-8 Bash payload with Base64 encoding can preserve multiline quoting across PowerShell and SSH; see [shell boundaries](create.md#connect-and-verify). Encoding is not encryption and must not carry secrets or unreviewed strings.

<a id="gpu-01"></a>
## GPU-01 — typed GRES versus node feature

A GPU model label may be a node feature rather than a typed GRES. Check current service documentation, partition inventory, GRES, features and permissions before requesting a GPU. Do not infer typed syntax or queue access from a label or a successful CPU job.

<a id="job-01"></a>
## JOB-01 — pending dependency or ambiguous acceptance

Check both queue and accounting and use the known job IDs or compare the requested job name, time and metadata. If it exists, use the existing job; do not submit again. For a pending dependency, inspect upstream state and exit code. Unclear or conflicting evidence is a stop, not an automatic resubmit. For a failed job, inspect job and step exit codes and the first relevant error before offering changes.
