---
tags: [gerrychain, class, analysis]
---

# Diversity Stats

**Module:** `gerrychain/meta/diversity.py`
**Class:** `DiversityStats`
**Part of:** [[GerryChain Architecture]]

`DiversityStats` measures **how diverse an ensemble is** — i.e. whether a [[MarkovChain]] run is actually exploring many distinct plans or churning over a few. This is a practical diagnostic for whether a chain has mixed well.

## What it tracks

`DiversityStats` records counts such as the number of **unique plans** seen, the number of **unique districts**, and how often plans repeat over the course of a run.

## Usage

`collect_diversity_stats(chain)` wraps a [[MarkovChain]] (or any iterable of [[Partition|partitions]]), yielding each partition while accumulating a `DiversityStats` summary you can inspect afterward:

```python
from gerrychain.meta.diversity import collect_diversity_stats

for partition, stats in collect_diversity_stats(chain):
    ...
# stats now summarizes how much unique structure the chain explored
```

## Why it matters

A chain that revisits the same handful of plans has not mixed, and any [[Metrics|metric]] distribution drawn from it is unreliable. Diversity stats give a quick sanity check before trusting an ensemble.

## See also
- [[MarkovChain]] — the run being measured
- [[Metagraph]] — formal view of the state space being explored
- [[GerryChain Architecture]]
