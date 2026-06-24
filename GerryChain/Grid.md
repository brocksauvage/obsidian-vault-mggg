---
tags: [gerrychain, class, core]
---

# Grid

**Module:** `gerrychain/grid.py`
**Parent class:** [[Partition]]
**Part of:** [[GerryChain Architecture]]

`Grid` is a [[Partition]] of an `m × n` grid graph. It exists for **testing, teaching, and quick experiments** — a synthetic geography where you can reason about results by hand, without loading real shapefiles.

## Construction

```python
Grid(dimensions=(10, 10), with_flips=True, ...)
```
It builds an underlying grid [[Graph]] (`_create_grid_nx_graph`) whose nodes are `(row, col)` tuples, optionally with diagonal edges. Each node is given constant attributes (e.g. `population`, `area`) and boundary nodes/perimeters are tagged so geometric updaters work. Default updaters include `cut_edges`, population/area tallies, and boundary/perimeter updaters (mirroring [[GeographicPartition]]).

## Seed colorings

Module helpers create starting assignments:
- `color_half(node, threshold)` — split the grid into two halves.
- `color_quadrants(node, thresholds)` — split into four quadrants.

## Display

- `as_list_of_lists()` returns the assignment as a 2-D grid for easy printing.
- `__str__` / `__repr__` render the district layout.

## See also
- [[Partition]], [[GeographicPartition]] — the behavior `Grid` inherits
- [[GerryChain Architecture]]
