---
tags: [gerrychain, class, updaters]
---

# DataTally

**Module:** `gerrychain/updaters/tally.py`
**Part of:** [[GerryChain Architecture]] · [[Updaters]]

`DataTally` is an [[Updaters|updater]] that tallies **numerical data per district** where the data is not necessarily stored as a node attribute. It is the lower-level cousin of [[Tally]].

## Usage

```python
DataTally(data, alias)
```

| Parameter | Meaning |
|---|---|
| `data` | A `dict` / `pandas.Series` indexed by the graph's nodes, **or** a string key naming a node attribute. |
| `alias` | The key under which the result is stored in the partition's `updaters`. |

This flexibility lets you tally values computed on the fly (not baked into the [[Graph]]). Internally [[Election]] builds one `DataTally` per party to track vote totals.

Like [[Tally]], it updates incrementally via the [[Updaters|flow]] machinery rather than re-summing every node each step.

## See also
- [[Tally]] — the node-attribute version
- [[Election]] — uses `DataTally` per party
- [[Updaters]]
