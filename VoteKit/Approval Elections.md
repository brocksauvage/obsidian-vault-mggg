---
tags:
  - votekit
---

# Approval Elections

Election types based on **approval ballots** — voters approve any number of candidates (a score ballot with 0/1 scores). Import from `votekit.elections`.

## Classes

| Class | Rule | Theory note |
|---|---|---|
| `Approval` | Approval voting — each approved candidate gets one point; highest totals win | — |
| `BlockPlurality` | Multi-winner block voting: approve up to *m* candidates for *m* seats | [[Block Plurality]] |

> [!note] `BlockPlurality` vs `BlocPlurality`
> Both names are exported from `votekit.elections`. `BlocPlurality` is a legacy spelling kept for compatibility; the API reference documents `BlockPlurality`. There are also internal `block_plurality` implementations under both the ranking and score modules, but the public approval-based `BlockPlurality` is the one to use.

## Example

```python
from votekit.elections import Approval, BlockPlurality

appr = Approval(score_profile, m=2)
block = BlockPlurality(score_profile, m=3)
```

## See also
- [[Ranking Elections]] · [[Score Elections]] · [[Elections Overview]]
- Theory: [[Block Plurality]], [[Plurality]]
