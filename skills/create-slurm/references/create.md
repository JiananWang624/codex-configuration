# KCL CREATE and Slurm

[Official access](https://docs.er.kcl.ac.uk/CREATE/hpc/access/) · [SSH clients](https://docs.er.kcl.ac.uk/CREATE/tools/ssh_clients/) · [Slurm and monitoring](https://docs.er.kcl.ac.uk/CREATE/hpc/running_jobs/)

## Connect and verify

Use the verified alias configured on the current device. A new device needs its own authorized access; do not copy private keys or guess an account. Use existing verified OpenSSH configuration and site-prescribed key/MFA flow. A bounded read-only check, when authorized and set up, is `ssh -o BatchMode=yes -o StrictHostKeyChecking=yes -o ConnectTimeout=10 -o ConnectionAttempts=1 <alias> hostname`. An authorized interactive login uses `ssh <alias>`; if its MFA request is pending, let the user approve in the trusted interface and wait for that same process. Do not solicit codes, initiate duplicate MFA, retry exited auth, or bypass host verification. A connection denied at TCP/22 is not a public-key rejection; if the execution environment restricts network access, report that limit without concluding the device, account, or service is broken. Do not retry using different credentials. Recheck hostname and authorized account locally without exposing the account more broadly.

**Shell boundaries:** Remote commands use Bash on CREATE. Preserve quoted remote `$USER` for expansion there, not in PowerShell. On Bash, an inspected LF script can be fed as `ssh "$alias" 'bash -s' < inspected.sh` for the requested read-only commands. On Windows for complex quoting, prepare an inspected LF UTF-8 Bash payload in a single-quoted here-string, convert with `[Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes($payload))`, and pass it as one argument to a remote decoder/Bash: `ssh $CreateAlias "printf '%s' '$encoded' | base64 -d | bash"` (where `$encoded` is the conversion output). Encoding is not encryption. Never encode secrets or unreviewed strings; no remote writes unless authorized.

Login nodes are for light inspection and submission, not training, simulation or CUDA workloads. Confirm project and scratch mounts with `realpath`/`test -d`. Do not store bulk data, checkpoints or caches in home. Do not assume paths, quotas, partitions or permissions from another account/device.

## Resources, queue, status and logs

Examples run on the verified remote login shell, not locally. Replace placeholders with validated values.

| Goal | Read-only starting point |
|---|---|
| Partitions and GRES | `sinfo -o '%P %a %l %D %G'`; for relevant GPU nodes `sinfo -N -p <partition> -o '%N %t %G %f'`. |
| My jobs | `squeue -u "$USER"`. |
| One existing job | `squeue -j <job-id>` for current queue; `sacct -j <job-id> --format=JobID,JobName,State,ExitCode,Elapsed,Timelimit,MaxRSS,ReqTRES,AllocTRES` for job/step history; `scontrol show job <job-id>` for active details and paths. An absent queue entry is not failure. |
| Logs | Resolve the recorded stdout/stderr file for this job/array task; use `tail -n 100 -- "<exact-log-path>"`. Preserve spaces and distinguish job from array-task output. Do not recursively search storage or follow indefinitely. |

For SCHEDULER dependencies or uncertain acceptance, use queue plus accounting and the known job name/time/IDs before acting. Check upstream states and exit codes, and do not infer either success or failure solely from pending states. See [JOB-01](create-issues.md#job-01) for decision guidance.

## Plan and submit once

1. Verify the current resource inventory and your account's permissions. Resolve the remote project/entrypoint and workdir; confirm dataset, checkpoint, cache and log paths, and environment. No heavy work during preflight. Capture enough code/config evidence to reconstruct the run.
2. Generate one reviewable Bash/Slurm batch script with explicit partition, CPU, GPU type/count if needed, memory, walltime, output/error paths and working directory. Keep the project command and arguments explicit and safely quoted; use only reviewed project initialization. Don't rely on inherited environment accidentally; any cache/checkpoint flags must also reach the actual project. CPU smoke test plans explicitly request CPU and **zero GPUs**: omit GPU/GRES directives. Request only resources needed for the task and do not increase the approved allocation. Verify site-specific GPU syntax and account eligibility; see [GPU-01](create-issues.md#gpu-01).
3. Dry-run means local text/argument/path-intent inspection and `bash -n` where Bash exists. Neither creates a job nor proves remote paths or scheduler acceptance. Show full batch content and the final command, resource counts, partition, walltime, workdir, data/cache/log/checkpoint paths, tradeoffs, and any uncertainty. Wait for the user's explicit confirmation of this exact submission. Changes to the plan require rechecking and reconfirming.
4. Never run `sbatch`, including `--test-only`, before that confirmation. After confirmation, run `sbatch --test-only <script>` on the verified host if supported, stop if not accepted, then submit **once** with `sbatch --parsable <script>`. Do not execute the batch locally, auto-retry, or mutate the reviewed plan silently. Check queue/accounting before considering a new submission if the original result is unclear.
5. Return verified Job ID, resources, and output/checkpoint locations; an initial status check is sufficient unless ongoing monitoring was requested and bounded. Cancellation requires separate explicit approval for the specific current job ID, name and state, then one `scancel <job-id>`; confirm resulting state. Do not cancel because an SSH session dropped.

## Triage after authentication

Separate scheduler status from transport. `FAILED` needs earliest relevant stderr/traceback and the job/step exit code; `OUT_OF_MEMORY` and step MaxRSS are not the same as evidence of CUDA OOM. `TIMEOUT` needs comparison with requested walltime and checkpoint resume support. For `CANCELLED`, check scheduler reason/actor where recorded rather than inferring who acted. Report evidence and requested changes; don't adjust scientific protocol/resources and resubmit as an implicit fix.
