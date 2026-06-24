---
tags:
  - votekit
aliases:
  - Cast Vote Records
  - CVR
---

# CVR Loaders

The **`votekit.cvr_loaders`** subpackage reads **real-world cast-vote-record (CVR) files** from disk into a [[Preference Profile]].

- **Module:** `votekit.cvr_loaders`

## Functions

| Function | Purpose |
|---|---|
| `load_csv` | General CSV loader (legacy/general entry point) |
| `load_ranking_csv` | Load a CSV of ranked ballots into a `RankProfile` |
| `load_scottish` | Parse the `.blt`-style format used by Scottish STV local elections |

## Typical use

```python
from votekit.cvr_loaders import load_ranking_csv

profile = load_ranking_csv("portland_2024.csv")
```

The resulting [[Preference Profile]] can then be passed straight into an [[Elections Overview|election]], optionally after [[Cleaning]].

> [!tip] Real data is messy
> Loaded profiles often contain skipped ranks, repeated candidates, overvotes, or write-ins. Run the [[Cleaning]] functions before analysis to normalize them.

## See also
- [[Ballot Generators]] — synthetic alternative to real CVRs.
- [[Cleaning]] · [[Preference Profile]]
- Used in VoteKit's Portland case-study tutorial.
