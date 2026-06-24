---
tags: [gerrychain, class, core]
---

# SubgraphView

**Module:** `gerrychain/partition/subgraphs.py`
**Part of:** [[GerryChain Architecture]]

`SubgraphView` provides access to the **induced subgraph of each district** within a [[Partition]], with a cache so subgraphs are not recomputed.

## How it works

It is constructed from a parent [[Graph]] and the `parts` of an [[Assignment]] (a mapping of district → node list):

```python
view = SubgraphView(graph, parts)
view[district_id]        # -> Graph induced on that district's nodes
for subgraph in view: ...
for part, subgraph in view.items(): ...
```

On first access, `view[part]` computes `graph.subgraph(nodes_in_part)` and stores it in `subgraphs_cache`; subsequent accesses return the cached subgraph. This matters because proposals like [[ReCom]] repeatedly need the subgraph of a merged pair of districts, and recomputation would be expensive.

A [[Partition]] exposes this as its `subgraphs` attribute.

## See also
- [[Partition]] — exposes a `SubgraphView` as `.subgraphs`
- [[Graph]] — provides `subgraph()`
- [[ReCom]] — a heavy consumer of district subgraphs
