---
tags: [gerrychain, metrics]
---

# Metrics

**Module:** `gerrychain/metrics/`
**Part of:** [[GerryChain Architecture]]

Metrics are the **scalar scores** used to evaluate a districting plan — the quantities you compute over an ensemble to decide whether the enacted plan is an outlier. There are two groups: **partisan** metrics and **compactness** metrics.

## Partisan metrics (`metrics/partisan.py`)

These take an `ElectionResults` object (from an [[Election]] updater) and return a single number measuring partisan fairness/bias.

| Function | Measures |
|---|---|
| `mean_median(results)` | Difference between the mean and median of a party's district vote shares — a simple skew indicator. |
| `mean_thirdian(results)` | Like mean-median but using the "thirdian." |
| [[Efficiency Gap\|`efficiency_gap(results)`]] | The net difference in **wasted votes** between parties, normalized by total votes — the headline gerrymandering metric. |
| `partisan_bias(results)` | Seats deviation from symmetry at a 50% vote share. |
| `partisan_gini(results)` | Gini-style measure of partisan asymmetry. |
| `wasted_votes(v1, v2)` | Helper: votes "wasted" by each party in a single district (feeds the efficiency gap). |

See [[Efficiency Gap]] for the most cited of these.

## Compactness metrics (`metrics/compactness.py`)

| Function | Measures |
|---|---|
| `compute_polsby_popper(area, perimeter)` | The [[Polsby-Popper Score]] for one district: `4πA / P²`. |
| `polsby_popper(partition)` | The Polsby-Popper score for every district in the [[Partition]]. |

These require geometric data, so they pair with [[GeographicPartition]].

## How metrics fit the workflow

1. Run a [[MarkovChain]] to build an ensemble.
2. For each [[Partition]], compute a metric (e.g. `efficiency_gap`).
3. Plot the distribution; locate the enacted plan within it.

The same metrics can act as the `optimization_metric` of a [[SingleMetricOptimizer]], or be wrapped in [[Bounds]] to become [[Constraints]].

## See also
- [[Efficiency Gap]], [[Polsby-Popper Score]]
- [[Election]] — source of partisan inputs
- [[GeographicPartition]] — source of compactness inputs
- [[SingleMetricOptimizer]], [[Bounds]]
