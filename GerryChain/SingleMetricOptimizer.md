---
tags: [gerrychain, class, optimization]
---

# SingleMetricOptimizer

**Module:** `gerrychain/optimization/optimization.py`
**Part of:** [[GerryChain Architecture]]
**Subclass:** [[Gingleator]]

Where a [[MarkovChain]] generates a *neutral ensemble*, `SingleMetricOptimizer` runs a chain to **search for plans that maximize (or minimize) a single score** — e.g. the most [[Efficiency Gap|efficiency-gap]]-favorable map, or the plan with the most majority-minority districts.

## Construction

```python
opt = SingleMetricOptimizer(
    proposal=my_recom,
    constraints=my_constraints,
    initial_state=partition,
    optimization_metric=lambda p: p["my_score"],
    maximize=True,
)
```

| Parameter | Meaning |
|---|---|
| `proposal` | Proposal function ([[ReCom]], etc.). |
| `constraints` | [[Validator]] or list of [[Constraints]]. |
| `initial_state` | Starting [[Partition]]. |
| `optimization_metric` | `Partition -> comparable` score to optimize. |
| `maximize` | Maximize (`True`) or minimize (`False`). |
| `step_indexer` | Updater name tracking step count (auto-added if missing). |

`best_part` and `best_score` hold the optimum found. **They reset at the start of each run** (they do not persist across multiple runs).

## Optimization strategies (each a generator over states)

| Method | Strategy |
|---|---|
| `short_bursts(...)` | Run short bursts of steps, restarting each burst from the best plan found so far. The standard, robust heuristic. |
| `simulated_annealing(...)` | Anneal an acceptance "temperature" so the walk explores early and exploits late. |
| `tilted_run(num_steps, p)` | Always accept improvements; accept worsening moves with probability `p`. |
| `tilted_short_bursts(...)` | Short bursts with a tilted acceptance rule. |
| `variable_length_short_bursts(...)` | Short bursts whose length changes over the run. |

### Cooling / beta schedules
For simulated annealing it provides several `beta`/temperature schedule functions: `jumpcycle_beta_function`, `linearcycle_beta_function`, `linear_jumpcycle_beta_function`, `logitcycle_beta_function`, `logit_jumpcycle_beta_function`. Internally these feed `_simulated_annealing_acceptance_function` and `_tilted_acceptance_function` (see [[Acceptance Functions]]).

## Usage pattern

```python
for partition in opt.short_bursts(burst_length=10, num_bursts=100):
    ...
best = opt.best_part
```

## See also
- [[Gingleator]] — subclass specialized for majority-minority districts
- [[MarkovChain]] — the underlying iteration model
- [[Acceptance Functions]], [[Metrics]], [[ReCom]]
