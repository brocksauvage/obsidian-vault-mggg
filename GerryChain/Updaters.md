---
tags: [gerrychain, updaters, moc]
---

# Updaters

**Module:** `gerrychain/updaters/`
**Part of:** [[GerryChain Architecture]]

**Updaters** are the functions that compute **derived statistics** about a [[Partition]] — population per district, cut edges, election results, perimeters, and so on. They are the bridge between raw node data and the [[Metrics]], [[Constraints]], and [[Acceptance Functions]] that consume them.

## The updater pattern

An updater is a `Partition -> result` function registered in a partition's `updaters` dict. Its result is accessed by subscripting and is **cached** per state:

```python
partition = Partition(graph, assignment, updaters={"population": Tally("TOTPOP")})
partition["population"]      # {district: total_population}
```

Every [[Partition]] includes `cut_edges` by default; [[GeographicPartition]] adds geometric updaters.

## Incremental updating — the performance core

The reason updaters exist as a framework (rather than plain functions) is **incremental computation**. When [[Partition|flip()]] creates a new state, only a few nodes changed. The `flows` machinery (`updaters/flows.py`) lets an updater recompute *just the delta* from its parent's cached value instead of from scratch:

- `flows_from_changes(old, new)` — what flowed in/out of each district.
- `on_flow(initializer, alias)` / `on_edge_flow(...)` — decorators that turn a "delta" function into a full updater that reuses the parent's value.
- `compute_edge_flows`, `neighbor_flips` — supporting computations.

This is what makes million-step chains tractable.

## Built-in updaters

| Updater | Module | Computes |
|---|---|---|
| [[Tally]] | `tally.py` | Sum of node attribute(s) per district (e.g. population). |
| [[DataTally]] | `tally.py` | Tally of data not stored as a node attribute. |
| `cut_edges`, `cut_edges_by_part` | `cut_edges.py` | Edges crossing district boundaries. |
| [[Election]] | `election.py` | Per-district vote totals & results for a modeled election. |
| `perimeter`, `boundary_nodes`, `exterior_boundaries`, `interior_boundaries` | `compactness.py` | Geometry for compactness scoring. |
| `county_splits`, `compute_county_splits` | `county_splits.py` | How many counties/regions a plan splits (with `CountySplit` enum). |
| `LocalitySplits` | `locality_split_scores.py` | Locality-splitting scores. |
| `num_spanning_trees` | `spanning_trees.py` | Spanning-tree counts per district. |

## See also
- [[Tally]], [[DataTally]], [[Election]]
- [[Partition]] — owns the updaters dict and the cache
- [[Metrics]], [[Constraints]] — consumers of updater output
