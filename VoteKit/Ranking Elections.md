---
tags:
  - votekit
aliases:
  - RankingElection
---

# Ranking Elections

Election types that consume **ranked ballots** (`RankBallot`s). All subclass `RankingElection` and follow the [[Elections Overview|Election workflow]]. Import from `votekit.elections`.

## Single-winner / classic
| Class | Rule | Theory note |
|---|---|---|
| `Plurality` | Most first-place votes win | [[Plurality]] |
| `SNTV` | Single non-transferable vote (multi-winner plurality) | [[Limited]] |
| `Borda` | Positional point scoring | [[Positional]] |
| `IRV` | Instant-runoff (single-winner [[Single Transferable Vote\|STV]]) | [[Ranked Choice Voting]] |
| `TopTwo` | Runoff between the top two | — |
| `Alaska` | Top-N primary then IRV | [[Ranked Choice Voting]] |

## Multi-winner transfer-based
| Class | Rule |
|---|---|
| `STV` | [[Single Transferable Vote]] (proportional RCV) |
| `FastSTV` | Performance-optimized STV (numpy backend) |
| `SequentialRCV` | Repeated single-winner RCV for multiple seats |

STV uses **transfer functions** (`fractional_transfer`, `random_transfer`) — see [[Elections Overview#Transfers]].

## Condorcet / pairwise family
Rules built on head-to-head comparisons ([[Pairwise Comparison]]):
| Class | Rule |
|---|---|
| `RankedPairs` | Tideman's ranked pairs |
| `Schulze` | Schulze (beatpath) method |
| `CondoBorda` | Condorcet winner, else [[Positional\|Borda]] |
| `DominatingSets` | Smith/dominating-set based |

## Veto & probabilistic
| Class | Rule |
|---|---|
| `PluralityVeto` | Plurality with a veto stage |
| `SerialVeto` | Sequential vetoing |
| `SimultaneousVeto` | Simultaneous vetoing |
| `RandomDictator` | Pick a random ballot's top choice |
| `BoostedRandomDictator` | Weighted/boosted variant |

## Example

```python
from votekit.elections import STV, Borda

stv = STV(profile, m=3)          # 3 seats
borda = Borda(profile)           # single winner by Borda score
```

## See also
- [[Score Elections]] · [[Approval Elections]] · [[Elections Overview]]
- Theory: [[Plurality]], [[Ranked Choice Voting]], [[Single Transferable Vote]], [[Positional]], [[Pairwise Comparison]]
