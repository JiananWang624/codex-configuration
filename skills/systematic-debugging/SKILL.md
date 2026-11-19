---
name: systematic-debugging
description: "Use when a bug, regression, performance drop, flaky or stochastic failure, numerical anomaly, robotics/simulation mismatch, or research-reproduction divergence has an unclear cause. Skip mechanical fixes whose cause is already established."
---

# Systematic Debugging

Diagnose uncertain causes with the cheapest faithful evidence. Preserve relevant controls, test explanations instead of patches, fix narrowly, and stop when the cause is supported.

## 1. Anchor the symptom

State observed versus expected behavior on the user's real failure surface. Capture the smallest useful evidence already available: error/output, failing command, benchmark delta, divergent metric, trace, reproduction rate, or known-good state. Do not substitute an easier nearby failure. For stochastic or intermittent failures, characterize frequency or distribution when one run is not representative.

## 2. Build a faithful signal and localize

Use the cheapest repeatable check that preserves the suspected mechanism: focused test/command, benchmark, replay, differential run, reduced simulation, or small harness. For expensive failures, prefer a faithful proxy or bounded real run over an artificial tiny repro.

Localize the fault domain with evidence: environment/build/hardware; data/version; config/evaluation protocol; interface/state/lifecycle; implementation/algorithm; concurrency/timing/distribution; numerical stability; or performance/resources.

For stochastic work, use repeated trials, seeds, or controlled conditions when needed. For robotics/simulation, preserve implicated timing, reset/initial state, control frequency, simulator/firmware/environment version, coordinate frames, and hardware boundary.

## 3. Discriminate causes

Keep only hypotheses that change the next action. Rank plausible causes and attach an observation that separates them. Use the smallest discriminating probe: bisection, differential runs, instrumentation/debuggers, profiling, or controlled perturbations. Change one material variable at a time when practical.

For research code, preserve experimental controls. Do not erase a reproduction mismatch by changing unrelated hyperparameters, datasets, metrics, checkpoint selection, or evaluation conditions. Verify authoritative documentation/source or direct evidence before adding a workaround based on assumed framework, driver, or hardware behavior.

## 4. Fix narrowly and verify

Apply the smallest change supported by evidence; avoid unrelated refactors, upgrades, or cleanup. After the last relevant mutation, rerun the original or strongest faithful reproduction and add a regression check when a stable representative seam exists.

Stop once the cause is sufficiently supported and the fix passes. Report root cause, discriminating evidence, fix, and residual uncertainty.

`research-reproduction` may use this skill for unexplained divergence. It diagnoses cause; it does not decide the reproduction verdict or automatically invoke `independent-verification`.
