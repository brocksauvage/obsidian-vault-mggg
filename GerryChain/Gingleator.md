---
tags: [gerrychain, class, optimization, vra]
---

# Gingleator

**Module:** `gerrychain/optimization/gingleator.py`
**Parent class:** [[SingleMetricOptimizer]]
**Part of:** [[GerryChain Architecture]]

`Gingleator` is a [[SingleMetricOptimizer]] specialized to **search for plans with more "Gingles" districts** — districts that are *majority-minority* (50% + 1 of some minority population subgroup).

> [!info] Why "Gingles"?
> The name comes from the Supreme Court case *Thornburg v. Gingles*. Demonstrating that **additional majority-minority districts could be drawn** is one of the litmus tests in bringing a Voting Rights Act (VRA) §2 claim. `Gingleator` automates that search.

## Construction

```python
Gingleator(
    proposal, constraints, initial_state,
    minority_perc_col=None,     # updater: district -> minority share
    threshold=0.5,              # share above which a district "counts"
    score_function=None,        # how to score a plan (see below)
    minority_pop_col=None,      # used to build minority_perc_col if absent
    total_pop_col="TOTPOP",
)
```
If you don't supply a `minority_perc_col` updater, it constructs one from `minority_pop_col` / `total_pop_col`.

## Scoring functions (class methods)

The optimizer needs a score telling it how close a plan is to gaining another opportunity district. `Gingleator` offers several:

| Score function | Idea |
|---|---|
| `num_opportunity_dists` | Count districts over the `threshold`. |
| `reward_partial_dist` | Count, plus partial credit for the closest below-threshold district. |
| `reward_next_highest_close` | Reward when the next district is *close* to flipping over. |
| `penalize_maximum_over` | Discourage "packing" any single district far past the threshold. |
| `penalize_avg_over` | Penalize the average over-concentration. |

These smoother scores help the [[SingleMetricOptimizer|short-burst]] search climb toward an extra opportunity district instead of getting stuck on the integer count.

## See also
- [[SingleMetricOptimizer]] — inherited optimization strategies
- [[Election]], [[Metrics]] — related quantitative VRA tooling
- [[GerryChain Architecture]]
