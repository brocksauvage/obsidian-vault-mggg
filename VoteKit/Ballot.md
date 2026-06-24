---
tags:
  - votekit
  - core-type
aliases:
  - RankBallot
  - ScoreBallot
---

# Ballot

The **`Ballot`** is the atomic unit of data in VoteKit. It represents a single cast vote (or a group of identical votes via `weight`).

- **Module:** `votekit.ballot`
- **Public classes:** `Ballot`, `RankBallot`, `ScoreBallot` (all importable from top-level `votekit`)

## The two subtypes

`Ballot` is a dispatching parent class: constructing one with a `ranking` returns a **`RankBallot`**, and constructing with `scores` returns a **`ScoreBallot`**. You cannot supply both.

| Subtype | Holds | Used by |
|---|---|---|
| `RankBallot` | an ordered `ranking` | [[Ranking Elections]] |
| `ScoreBallot` | a `scores` dict | [[Score Elections]], [[Approval Elections]] |

## Attributes

- **`ranking`** (`tuple[frozenset[str], ...] | None`) — entry *i* is the set of candidates in rank position *i*. Using a **frozenset** per position lets a ballot express ties (multiple candidates at the same rank).
- **`scores`** (`dict[str, float] | None`) — maps candidate → score. Only non-zero scores are retained.
- **`weight`** (`float`, default `1.0`) — how many identical ballots this object stands for. Coerced to float; cannot be negative.
- **`voter_set`** (`frozenset[str]`) — optional set of voters who cast the ballot.

## Design notes

- **Immutable & memory-light.** Uses `__slots__` and is frozen after `__init__` (`_frozen`). This makes ballots hashable and lets a [[Preference Profile]] store one object per *distinct* ballot, with `weight` recording multiplicity — important for large electorates.
- **Ties via frozensets.** Because each rank position is a frozenset, the same structure naturally encodes both strict rankings and ties; raises `TypeError` if both `ranking` and `scores` are given.

## Example

```python
from votekit import Ballot

# A ranked ballot: A first, B and C tied second
b = Ballot(ranking=[{"A"}, {"B", "C"}], weight=3)

# A scored ballot (e.g. for cumulative/rating)
s = Ballot(scores={"A": 2, "B": 1})
```

## See also
- [[Preference Profile]] — collections of ballots.
- [[Cleaning]] — operations that add/remove candidates or condense ballots.
