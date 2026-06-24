---
tags: [gerrychain, class, core, graph]
---

# FrozenGraph

**Module:** `gerrychain/graph/graph.py`
**Part of:** [[GerryChain Architecture]]

`FrozenGraph` is an **immutable, cached wrapper** around a [[Graph]]. A [[Partition]] stores its graph as a `FrozenGraph` so that, during a long [[MarkovChain]] run, the structure cannot be mutated and expensive lookups are memoized.

## Why freeze the graph?

Across a chain, the **graph never changes** — only the [[Assignment]] of nodes to districts does. Freezing the graph:

1. **Prevents accidental mutation** of shared structure across the millions of states.
2. **Enables caching** of frequently-requested values (e.g. neighbor lists, degrees) without worrying about staleness.

It guards mutation through `__getattribute__`, and caches results for accessors like `neighbors(n)`, `degree(n)`, `node_indices`, and `edge_indices`. `subgraph(nodes)` returns a new `FrozenGraph` for a district's nodes.

## Relationship to Graph

`FrozenGraph` delegates to the wrapped [[Graph]] for anything it does not override, so from the perspective of updaters and proposals it looks like a `Graph`. The plain [[Graph]] class is what you load and edit; `FrozenGraph` is what the chain runs on.

## See also
- [[Graph]] — the mutable, loadable graph it wraps
- [[Partition]] — stores its graph as a `FrozenGraph`
