---
name: systematic-debugging
description: "Diagnose failures with unclear causes, including regressions, performance drops, intermittent failures, numerical anomalies, and robotics, simulation, or research reproduction mismatches. Skip established causes and mechanical fixes."
---

# Systematic Debugging

## Establish a faithful signal

State observed and expected behavior using the most useful evidence already available. Choose the least costly check that preserves the conditions relevant to the failure, and use it to narrow the fault domain. For intermittent failures, characterize frequency or variability within the agreed budget and keep comparison conditions comparable.

## Discriminate causes

Keep hypotheses that would change the next action. Choose the smallest probe and state which result would distinguish them. Use bisection, instrumentation, profiling, or controlled comparisons as appropriate. Verify assumptions about frameworks, drivers, simulators, or hardware before basing a workaround on them.

## Preserve domain controls

For research reproduction, hold data, metrics, checkpoint selection, evaluation conditions, and unrelated settings fixed. For robotics or simulation, preserve relevant reset state, timing, control frequency, simulator or firmware versions, coordinate frames, and the boundary between simulation and hardware.

## Finish at the requested scope

Report the cause supported by the evidence. If the evidence is inconclusive, report what remains uncertain and the next discriminating check.

If a fix is in scope, make the smallest change supported by the evidence. Rerun the original reproduction after the final relevant change. If only a faithful proxy is feasible, report what it verified and what remains untested. Add a regression check when a stable case covers a gap in existing tests.

For research reproduction, report the diagnostic evidence and leave the overall verdict to the reproduction task.
