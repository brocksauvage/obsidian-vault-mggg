---
tags:
  - votekit
---

# Score Elections

Election types that consume **scored ballots** (`ScoreBallot`s), where voters assign numeric scores to candidates. Import from `votekit.elections`.

## Classes

| Class | Rule | Theory note |
|---|---|---|
| `GeneralRating` | General framework: each voter distributes scores under configurable per-candidate and total limits | — |
| `Rating` | Score/range voting — independent score per candidate | — |
| `Cumulative` | Voters spread a fixed budget of votes, may pile them on one candidate | [[Cumulative]] |
| `Limited` | Fewer votes than seats, at most one per candidate | [[Limited]] |

`GeneralRating` is the parent abstraction; `Rating`, `Cumulative`, and `Limited` are specific constraint settings on how scores may be allocated.

## Relationship to ballots & generators

- Input is a `ScoreProfile` of `ScoreBallot`s — see [[Preference Profile]] and [[Ballot]].
- Synthesize cumulative-style ballots with `name_cumulative_profile_generator` — see [[Ballot Generators]].
- A ranked profile can be converted to scores via `convert_rank_profile_to_score_profile_via_score_vector` (e.g. apply a [[Positional|Borda]] vector).

## Example

```python
from votekit.elections import Cumulative

election = Cumulative(score_profile, m=3)
```

## See also
- [[Ranking Elections]] · [[Approval Elections]] · [[Elections Overview]]
- Theory: [[Cumulative]], [[Limited]]
