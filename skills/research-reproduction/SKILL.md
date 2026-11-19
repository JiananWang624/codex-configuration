---
name: research-reproduction
description: "Use when executing or validating a computational result tied to an academic paper or official research artifact: reproduce or replicate a table, figure, benchmark, algorithm, system, simulation, or establish a baseline before an extension. Do not use for general paper discussion, literature review, or idea generation."
---

# Research Reproduction

Reproduce the requested scientific claim, not merely a runnable program. Preserve variables that control the claim and keep published facts separate from local decisions.

## 1. Fix the reproduction contract

Before expensive execution, establish the exact claim and primary-source location; expected artifact/metric/behavior; materially controlling protocol variables; published comparison criterion (or raw difference/uncertainty if none exists); and user scope/resource constraints.

Prefer the paper/supplement, authors' official repository, released configs/data/artifacts, and authoritative project documentation. Do not silently expand the target to the whole paper.

## 2. Preserve provenance and establish the baseline

Track only provenance that can change interpretation: paper/version, repository commit/tag and dirty state, data/artifact version, config/command, materially relevant runtime/dependencies/hardware, and protocol settings.

Keep **reported** (primary source), **observed** (current execution), **assumed** (source underspecified), and **local choice** (engineering decision outside the published method) distinct. Record disagreements among paper, supplement, README, code, config, or released artifacts instead of silently choosing one.

Prefer the authors' released execution path before rewriting, refactoring, retuning, or extending. Reuse a baseline only if materially relevant code, data, protocol, and state are unchanged; otherwise run a cheap faithful smoke check, then the requested baseline. Do not change the research variable until the baseline is trustworthy enough for comparison.

## 3. Execute the claim-specific protocol

Preserve what controls the claim:

- **ML/RL:** material seeds/repetitions, train/eval randomness, checkpoint selection, evaluation count, policy sampling, preprocessing/normalization, action scaling, termination/truncation.
- **Robotics/simulation:** simulator/firmware/environment versions, simulation/control frequencies, reset/initial conditions, timing, coordinate frames, randomization, and relevant hardware boundaries.
- **Systems/performance:** workload, warm-up, concurrency, hardware, resource limits, measurement window.
- **Algorithms/numerics:** inputs, precision, tolerances, initialization, stopping criteria.

A successful process exit is not a reproduced result. Validate the contract's artifact or measurement. Match the paper's aggregation/statistical convention when available; do not promote a convenient single stochastic run into a population-level match unless the published protocol is single-run.

## 4. Handle divergence and conclude

Before changing the method, classify divergence as source/version, data/artifact, environment/hardware/dependency, config/evaluation protocol, implementation defect, stochastic/statistical variation, numerical precision, or resource/incomplete-run effects. Test the smallest discriminating change while preserving controls; use `systematic-debugging` when the cause is unclear. Do not tune unrelated parameters merely to move the result toward the paper.

If an extension is requested, start only from a sufficiently trustworthy baseline and isolate the research variable.

Finish with the narrowest truthful status: **REPRODUCED**, **APPROXIMATELY_REPRODUCED**, **PARTIALLY_REPRODUCED**, **NOT_REPRODUCED**, **INCONCLUSIVE**, or **BLOCKED**. Report observed versus reported results, material provenance/protocol, assumptions/local choices, investigated discrepancies, and remaining uncertainty. Do not promote a match into broader claims about causality, generalization, novelty, or scientific truth.

Do not automatically invoke `independent-verification`; it is a separate user-requested audit.
