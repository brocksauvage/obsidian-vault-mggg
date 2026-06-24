---
tags: [gerrychain, class, updaters]
---

# Tally

**Module:** `gerrychain/updaters/tally.py`
**Part of:** [[GerryChain Architecture]] · [[Updaters]]

`Tally` is the most-used [[Updaters|updater]]: it **sums one or more node attributes within each district**. Population, vote totals, and area are all tallies.

## Usage

```python
from gerrychain.updaters import Tally

updaters = {
    "population": Tally("TOTPOP", alias="population"),
    "VAP": Tally(["WVAP", "BVAP", "HVAP"], alias="VAP"),  # sum several columns
}
partition = Partition(graph, assignment, updaters)
partition["population"]   # {district_id: summed_population}
```

| Parameter | Meaning |
|---|---|
| `fields` | A node-attribute name, or list of names to sum together. |
| `alias` | The key under which the result appears in `partition[...]` (defaults to the first field). |
| `dtype` | Numeric type of the tally (default `int`). |

## Incremental behavior

`Tally` updates incrementally using the [[Updaters|flow]] machinery: `compute_in_flow` / `compute_out_flow` add the attribute totals of nodes that entered/left each district, so a new state's tally is computed from its parent's, not recomputed over every node.

## [[DataTally]]

`DataTally` is the sibling class for tallying data that is **not** stored as a node attribute — you supply a dict / `pandas.Series` indexed by node (or an attribute key) plus an `alias`. See [[DataTally]].

## See also
- [[DataTally]], [[Updaters]]
- [[Election]] — built on top of `DataTally` tallies per party
- [[Partition]]
