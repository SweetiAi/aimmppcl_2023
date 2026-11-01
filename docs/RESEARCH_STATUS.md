# Research Status: Adaptive Planning Depth and Meta-Control

## Question

Can an Active Inference agent determine when additional predictive planning is worth its computational cost, and how deep that planning should be?

## Experimental intervention

From identical pre-planning checkpoints, the agent was cloned and evaluated under fresh DPEFE planning depths:

```text
N = 5, 10, 25, 50
```

The learned state was held fixed across counterfactual branches; planning depth was the intervention.

## What was established

- Planning depth materially changes the DPEFE policy in difficult regimes.
- Deeper planning can substantially change behavioural success and capped episode length.
- The useful planning depth depends on learning regime / context.
- A single globally fixed planning depth is therefore not always computationally efficient.

## What was not established

The current study did **not** find reliable evidence that the tested pre-planning internal state can identify, beyond coarse phase/regime, when deeper planning is worth its cost.

Tested summaries included:

- CL policy entropy,
- cached DPEFE policy entropy,
- CL/cache disagreement,
- learned transition confidence,
- cache age.

A final diagnostic also compared richer representations using the full `C_1`, `Q_pi`, `Beta`, `Gamma`, and the full learned transition model `B`.

Under the strictest generalization test (holding out an entire base seed), these internal-state representations did not outperform a phase-based baseline in both independent split directions.

## Interpretation

The study supports:

```text
planning depth has context-dependent value
```

but does not support:

```text
tested internal state -> reliable checkpoint-level value-of-computation estimate
```

The key conceptual lesson is:

```text
context-dependent computation != metacognitive control of computation
```

## Status of the hypothesis

The current checkpoint-level meta-control hypothesis is considered **closed**. No additional feature search or simulation should be used solely to rescue the same hypothesis.

Future work should be framed as a new research question, with cheap falsification before large simulation.

## Reusable infrastructure

The codebase remains useful for future work because it implements:

- exact episode-level snapshots,
- state cloning,
- planning-depth counterfactual interventions,
- common-random-number evaluation,
- independent selection/test Monte Carlo splits,
- memoized fresh planning,
- parallel evaluation,
- resume/recovery support,
- integrity checks,
- policy sensitivity analysis,
- cost-sensitive Oracle comparisons,
- grouped out-of-sample validation.

## Strongest future directions

1. Explicit `REUSE CACHE` vs `FRESH PLAN` computational actions.
2. A latent meta-state for expected model reliability or value of computation.
3. Change-point / volatility inference.
4. Task designs where computational demand varies independently of experiment phase.
5. Cheap falsification and behavioural micro-pilots before any full screening run.
