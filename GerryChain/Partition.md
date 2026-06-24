---
tags: [gerrychain, class, core]
---

# Partition

**Module:** `gerrychain/partition/partition.py`
**Part of:** [[GerryChain Architecture]]
**Subclasses:** [[GeographicPartition]], [[Grid]]

A `Partition` represents a partition of the nodes of a [[Graph]] into districts — i.e. **a single state of the [[MarkovChain]]**. It performs the first layer of computation at each step: basic aggregations and cached statistics.

## What it holds

| Attribute | Meaning |
|---|---|
| `graph` | The underlying [[Graph]] (stored internally as a [[FrozenGraph]]). |
| `assignment` | An [[Assignment]] mapping node IDs → district IDs. |
| `parts` | Dict mapping district ID → set of node IDs in that district. |
| `subgraphs` | A [[SubgraphView]]: district ID → induced subgraph (lazy + cached). |
| `updaters` | Dict of functions computing derived data; results are cached per-state. See [[Updaters]]. |
| `parent` | The previous `Partition` this one was derived from (set to `None` mid-chain to avoid memory leaks). |
| `flips` | The `{node: new_district}` changes relative to `parent`. |
| `flows`, `edge_flows` | Incremental node/edge changes used to update updaters efficiently. |

`Partition` uses `__slots__` for memory efficiency, important when millions of states are created.

## Two ways to construct

**1. From scratch** (first state of a chain):
```python
partition = Partition(graph, assignment, updaters)
```
`assignment` may be a dict, a node-attribute name, or an [[Assignment]].

**2. By flipping** (every subsequent state):
```python
new_partition = partition.flip({node: new_district, ...})
```
`flip()` creates a child `Partition` that **reuses the parent's cached data** and only recomputes what the changed nodes affect. This incremental update is the key performance trick that makes long chains feasible.

## The updater mechanism

Each key in `updaters` becomes accessible by subscripting the partition:

```python
partition["population"]      # runs/returns the "population" updater
partition["cut_edges"]       # the default updater
```

By default every `Partition` includes the `cut_edges` updater (see [[Updaters]]). Pass `use_default_updaters=False` to omit it. Updaters are computed lazily and cached in `_cache`.

> [!tip] Why both `assignment` and `parts`?
> `assignment` answers "which district is this node in?" cheaply; `parts` answers "which nodes are in this district?" cheaply. [[Assignment]] keeps both views in sync so proposals and updaters can use whichever is faster.

## Relationship to the chain

A `Partition` is intimately tied to how the [[MarkovChain]] works: proposals produce new partitions via `flip()`, [[Constraints|constraints]] inspect them, and [[Updaters|updaters]] expose the statistics that [[Acceptance Functions|acceptance functions]] and [[Metrics|metrics]] consume.

## See also
- [[GeographicPartition]] — adds area/perimeter/boundary updaters for [[Polsby-Popper Score|compactness]]
- [[Assignment]], [[SubgraphView]], [[Graph]], [[FrozenGraph]]
- [[Updaters]], [[MarkovChain]]
