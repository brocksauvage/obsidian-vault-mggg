---
tags: [gerrychain, class, core]
---

# GeographicPartition

**Module:** `gerrychain/partition/geographic.py`
**Parent class:** [[Partition]]
**Part of:** [[GerryChain Architecture]]

A `GeographicPartition` is a [[Partition]] that additionally tracks **areas, perimeters, and boundary information**. These geometric quantities let you compute compactness scores such as the [[Polsby-Popper Score]].

## What it adds

It overrides `default_updaters` with a geometry-aware set (see [[Updaters]]):

| Updater | Provides |
|---|---|
| `perimeter` | Perimeter of each district. |
| `area` | `Tally("area")` — total area of each district. |
| `exterior_boundaries` | Boundary shared with the outside of the state. |
| `interior_boundaries` | Boundary shared between districts. |
| `boundary_nodes` | Nodes on the state's outer boundary. |
| `cut_edges` | Edges crossing district lines (also a [[Partition]] default). |
| `cut_edges_by_part` | Cut edges grouped by district. |

These require the underlying [[Graph]] to carry geometric node/edge data (typically loaded via `Graph.from_file` from a shapefile/GeoDataFrame, which populates `area` and shared-`perimeter` edge attributes).

## When to use it

Use `GeographicPartition` instead of `Partition` whenever your analysis or constraints involve geometry — for example a [[Polsby-Popper Score|Polsby-Popper]] compactness [[Bounds|bound]] or reporting district compactness across an ensemble.

```python
from gerrychain import GeographicPartition
partition = GeographicPartition(graph, assignment="CD", updaters={...})
partition["perimeter"], partition["area"]
```

## See also
- [[Partition]] — base behavior and the updater mechanism
- [[Polsby-Popper Score]], [[Metrics]] — compactness scoring
- [[Updaters]] — how the geometric updaters are computed
