---
tags: [gerrychain, acceptance]
---

# Acceptance Functions

**Module:** `gerrychain/accept.py`
**Part of:** [[GerryChain Architecture]]

An **acceptance function** is the `accept` argument of a [[MarkovChain]]: given a *valid* proposed [[Partition]], it returns `True` to **move** there or `False` to stay put. This is the Metropolis–Hastings step — it shapes the stationary distribution the chain samples from.

## Built-in functions

| Function | Behavior |
|---|---|
| `always_accept(partition)` | Always returns `True`. The default for **neutral ensemble generation** — every valid proposal is taken. |
| `cut_edge_accept(partition)` | Accepts with probability based on the ratio of cut edges between the proposed and current state, biasing toward more compact plans. |

## Writing your own

Any `Partition -> bool` works. A Metropolis–Hastings rule typically compares a score on the proposal vs. its `parent`:

```python
import random, math

def metropolis(partition):
    old = partition.parent["my_score"]
    new = partition["my_score"]
    return random.random() < min(1, math.exp(-(new - old) / T))
```

> [!note] Validity vs. acceptance
> [[Constraints]] (the [[Validator]]) decide whether a state is *allowed*; the acceptance function decides whether an *allowed* state is *moved to*. The [[SingleMetricOptimizer]] supplies its own specialized acceptance functions (tilted, simulated-annealing) to bias the walk toward high-scoring plans.

## See also
- [[MarkovChain]] — consumes the `accept` argument
- [[Constraints]], [[Validator]] — the separate validity gate
- [[SingleMetricOptimizer]] — optimization-oriented acceptance rules
