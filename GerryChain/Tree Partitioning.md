---
tags: [gerrychain, trees]
---

# Tree Partitioning

**Module:** `gerrychain/tree.py`
**Part of:** [[GerryChain Architecture]]

`tree.py` contains the **spanning-tree machinery** that powers [[ReCom]] and the generation of initial seed plans. The central idea: to split a region into population-balanced districts, build a spanning tree of it and cut edges so the resulting pieces hit a target population.

## Building spanning trees

- `random_spanning_tree(graph, region_surcharge=None)` — a random spanning tree (Kruskal/Karger), optionally weighting edges so **region-aware** cuts avoid crossing county/municipality lines.
- `uniform_spanning_tree(graph, choice=random.choice)` — a *uniformly* random spanning tree (Wilson's algorithm), used when unbiased sampling matters.

## Finding balanced cuts

Given a populated tree, find edges whose removal yields a piece with the target population:

- `find_balanced_edge_cuts_contraction(...)` — by contracting the tree.
- `find_balanced_edge_cuts_memoization(...)` — a memoized (faster) version.

## Bipartitioning

- `bipartition_tree(graph, pop_col, pop_target, epsilon, ...)` — the default splitting `method` used by [[ReCom]]: build a tree, find a balanced cut, return the two parts. Retries up to `max_attempts`.
- `bipartition_tree_random(...)` and `bipartition_tree_random_with_num_cuts(...)` — random variants.
- `epsilon_tree_bipartition(...)` — bipartition within an `epsilon` population tolerance (what `recom` calls internally).

## Seeding whole plans

- `recursive_tree_part(graph, parts, pop_target, pop_col, epsilon, ...)` — recursively bipartitions to produce a full `k`-district plan. **This is how you generate an initial seed [[Partition]]** when you don't have a real starting map. It carries a running population "debt" so the *last* district also lands within `epsilon`.
- `recursive_seed_part(...)` / `_recursive_seed_part_inner(...)` — a seeding variant that splits into prime-factor-sized chunks for more balanced recursion.

## [[PopulatedGraph]]

A helper class wrapping a tree together with per-node population, used while searching for balanced cuts. See [[PopulatedGraph]].

## Exceptions & warnings

`BalanceError`, `PopulationBalanceError`, `BipartitionWarning`, and `ReselectException` (the signal [[ReCom]] uses to retry a different district pair).

## See also
- [[ReCom]] — the main consumer
- [[PopulatedGraph]], [[Graph]]
- [[Partition]] — `recursive_tree_part` output is used to build the initial state
