---
tags: [gerrychain, class, updaters, elections]
---

# Election

**Module:** `gerrychain/updaters/election.py`
**Part of:** [[GerryChain Architecture]] · [[Updaters]]

`Election` is an [[Updaters|updater]] representing **one election**, with a hypothetical race re-tabulated in each district as the plan changes. This is how GerryChain answers "how many seats would party X win under this map?" across an entire ensemble.

## The idea

Node-level vote totals are fixed; only the [[Assignment]] of nodes to districts varies. Rather than recompute per-party tallies and percentages from scratch each step, `Election` manages a [[DataTally]] per party and produces an `ElectionResults` view.

## Defining an election

```python
from gerrychain import Election

election = Election(
    "2008 Senate",
    {"Democratic": "2008_D", "Republican": "2008_R"},   # party -> node attribute
    alias="2008_Sen",
)

partition = Partition(graph, assignment, updaters={"2008_Sen": election})
partition["2008_Sen"].wins("Republican")
```

| Parameter | Meaning |
|---|---|
| `name` | Human-readable name (e.g. "2008 Presidential"). |
| `party_names_to_node_attribute_names` | Maps each party to the node attribute holding its vote totals (dict or list). |
| `alias` | Key under which the election is registered in the partition's updaters. |

## The supporting classes

- **`ElectionUpdater`** — the actual updater object the partition calls. It maintains the per-party [[DataTally|tallies]] and returns an `ElectionResults`.
- **`ElectionResults`** — a convenient view of the outcome, with query methods such as:
  - `wins(party)` — number of districts won by a party.
  - `percent(party, district=...)` — vote share.
  - `votes(party)`, `counts(party)`, `totals()` — raw numbers.
  - `seats(party)` and the data feeding partisan [[Metrics]].

`ElectionResults` is exactly what the partisan [[Metrics]] functions (mean-median, [[Efficiency Gap]], partisan bias/gini) take as input.

## See also
- [[Metrics]] — partisan metrics computed from `ElectionResults`
- [[DataTally]], [[Tally]], [[Updaters]]
- [[Partition]]
