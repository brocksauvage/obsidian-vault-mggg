---
tags: [gerrychain, class, proposals]
---

# ReCom

**Module:** `gerrychain/proposals/tree_proposals.py`
**Part of:** [[GerryChain Architecture]] · [[Proposals]]

**ReCom** (short for **ReCombination**) is the workhorse [[MarkovChain]] proposal for redistricting. It exists both as a function `recom(...)` and as a configurable class `ReCom`.

## The algorithm

At each step:

1. Pick a **pair of adjacent districts** at random (via a shared cut edge).
2. **Merge** them into one region.
3. Build a **random spanning tree** of that region (Kruskal/Karger style — see [[Tree Partitioning]]).
4. **Cut one edge** of the tree to split the region back into two districts.
5. Accept the cut only if both new districts are **population-balanced** within `epsilon`; otherwise pick another edge / pair and retry.

Because each move re-draws a whole pair of districts at once, ReCom takes large steps and produces a well-mixed ensemble far faster than single-node flip [[Proposals|flips]].

## The `recom` function

```python
from functools import partial
from gerrychain.proposals import recom

pop_target = sum(partition["population"].values()) / len(partition)
proposal = partial(
    recom, pop_col="POP10", pop_target=pop_target, epsilon=0.05, node_repeats=10
)
chain = MarkovChain(proposal, constraints, accept, partition, total_steps)
```

| Parameter | Meaning |
|---|---|
| `pop_col` | Node attribute holding population. |
| `pop_target` | Ideal population per district. |
| `epsilon` | Allowed deviation as a fraction of `pop_target`. |
| `node_repeats` | How many times to retry tree-cutting before reselecting the pair. |
| `region_surcharge` | Weights for **region-aware** ReCom (keep counties/municipalities whole). |
| `method` | The bipartitioning function (default `bipartition_tree`). See [[Tree Partitioning]]. |

If every adjacent district pair fails to split, it raises `MetagraphError` (try a different random seed).

## The `ReCom` class

`ReCom.__init__(pop_col, pop_target, epsilon, ...)` stores the configuration; calling the instance `recom_obj(partition)` runs one step. This is just an object-oriented wrapper around `recom` for convenient reuse.

## Reversible ReCom

`reversible_recom(...)` is a variant with well-defined transition probabilities (bounded edge selection), used when reversibility of the chain matters. Misconfiguration raises `ReversibilityError`.

## Region-aware ReCom

Passing `region_surcharge` (e.g. `{"county": 1.0}`) penalizes spanning-tree edges that cross a region boundary, so ReCom prefers splits that **keep counties/municipalities intact**. See the `tests/test_region_aware.py` for behavior.

## See also
- [[Tree Partitioning]] — `bipartition_tree`, `recursive_tree_part`, [[PopulatedGraph]]
- [[Proposals]] — the broader family of proposals
- [[Spectral Proposals]] — alternative splitting method
- [[MarkovChain]], [[Acceptance Functions]]
