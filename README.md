# Adaptive Planning Depth and Meta-Control in Active Inference

This branch extends the original Active Inference mixed-model implementation by **Aswin Paul and collaborators** to investigate a new question:

> **When — and how much — should an Active Inference agent plan?**

The original model combines two action-selection mechanisms:

- **Counterfactual Learning (CL)** — experience-based action selection.
- **Dynamic Programming Expected Free Energy (DPEFE)** — predictive planning.

The original model uses a state-dependent arbitration variable, `Beta`, to balance these already-computed action distributions. The present project asks a higher-level computational question: whether an agent can determine when additional predictive planning is worth its computational cost, and how deep that planning should be.

## Research question

We evaluated planning depths

```text
N ∈ {5, 10, 25, 50}
```

from identical pre-planning checkpoints. Each candidate depth was applied as a counterfactual intervention while keeping the underlying learned state fixed.

The central meta-control objective was treated as a value-of-computation problem:

```text
expected behavioural benefit - λ × computational cost
```

The long-term motivation is to distinguish two levels of control:

1. **Action arbitration:** how much to rely on CL versus DPEFE once both are available.
2. **Computational arbitration:** whether fresh planning should be performed, and if so, how deeply.

## Experimental framework

The project developed a reproducible counterfactual evaluation framework with:

- exact episode-level checkpointing,
- cloning of identical learned states,
- planning-depth-only interventions,
- common random numbers across counterfactual branches,
- independent selection and test Monte Carlo splits,
- fresh-plan memoization across rollouts,
- parallel evaluation,
- resume and recovery support,
- integrity checks for saved snapshots and outputs,
- policy-sensitivity analysis,
- cost-sensitive Oracle evaluation,
- grouped out-of-sample validation.

## Main findings

### 1. Planning depth matters

Planning depth substantially changed the DPEFE policy in the difficult environment. Differences between shallow and deep planning were small in the easy regime but became large after the environment changed.

### 2. Planning depth can strongly affect behaviour

In some hard-regime checkpoints, deeper planning substantially improved success and reduced capped episode lengths.

### 3. The value of planning is context-dependent

No single planning depth dominated every learning regime. Deeper planning was particularly valuable in some post-change phases, while shallow planning was sufficient elsewhere.

### 4. Context dependence did **not** yield reliable internal meta-control

We tested whether pre-planning internal signals could predict when deeper planning was worth its cost. Tested signals included:

- entropy of the CL action distribution,
- entropy of the cached DPEFE policy,
- CL/cache Jensen-Shannon disagreement,
- learned transition-model confidence,
- cache age,
- richer representations of the full pre-planning policy/arbitration state,
- the full learned transition model `B`.

These representations did **not** reliably outperform a simple learning-phase baseline when generalizing to an unseen base seed.

The final interpretation is therefore:

> **Planning depth had real, context-dependent computational value, but the tested pre-planning internal state did not reliably identify when deeper planning was worth its cost beyond coarse learning regime.**

This is treated as a negative result for the current checkpoint-level meta-control hypothesis, not as evidence that adaptive planning is generally impossible.

## Why the negative result is useful

The project separates two claims that are easy to conflate:

```text
context-dependent value of computation
        ≠
metacognitive control of computation
```

A computation can be more useful in some contexts without the current agent possessing an internal representation that lets it identify those contexts online.

## Planned repository structure

```text
experiments/oracle_planning_depth/   Counterfactual evaluation framework
analysis/                            Post-hoc and failure-diagnostic analyses
results/summary/                     Small aggregate result tables only
figures/                             Publication-quality visualizations
docs/                                Project report and methodological notes
tests/                               Equivalence / integrity / reproducibility tests
```

Large raw snapshots, transient runtime state, and heavy Monte Carlo artifacts are intentionally excluded from normal Git history.

## Recommended future direction

The current hypothesis is considered closed. A future project should be treated as a new research question rather than an attempt to rescue this result. Stronger directions include:

- **REUSE CACHE vs FRESH PLAN** as explicit computational actions,
- an explicit latent meta-state for expected model reliability or expected value of computation,
- change-point / volatility inference,
- redesigned tasks in which computational demand varies independently of experimental phase,
- cheap falsification and micro-behavioural tests before large simulations.

## Original model and attribution

This repository is based on the original mixed-model implementation by **Aswin Paul** and collaborators. The original code is distributed under the MIT License; the original copyright notice is retained in `LICENSE`.

The adaptive-planning experiments and analysis framework in this branch are an independent research extension built on top of that implementation.

## Project status

**Status:** completed exploratory / screening study.  
**Primary meta-control hypothesis:** not supported by Pilot 0.  
**Planning-depth causal effect:** supported.  
**Context-dependent value of planning:** supported.  
**Codebase:** retained as a reusable research framework.
