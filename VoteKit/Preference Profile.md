---
tags:
  - votekit
  - core-type
aliases:
  - PreferenceProfile
  - RankProfile
  - ScoreProfile
---

# Preference Profile

A **`PreferenceProfile`** is a collection of [[Ballot|ballots]] — VoteKit's representation of an entire electorate. It is the object you feed to an [[Elections Overview|election]].

- **Module:** `votekit.pref_profile`
- **Public classes:** `PreferenceProfile`, `RankProfile`, `ScoreProfile`, `CleanedRankProfile`, `CleanedScoreProfile`, and the error type `ProfileError` (top three importable from `votekit`).

## Variants

| Class | Contains | Notes |
|---|---|---|
| `RankProfile` | `RankBallot`s | Ranked elections |
| `ScoreProfile` | `ScoreBallot`s | Scored / approval elections |
| `CleanedRankProfile` / `CleanedScoreProfile` | cleaned ballots | Returned by [[Cleaning]]; track what was changed |

Profiles are backed by a pandas `DataFrame` internally, so VoteKit ships helpers like `profile_df_head` / `profile_df_tail` to peek at them.

## What a profile gives you

- The set of **candidates** and the collection of (distinct) ballots with their weights.
- Conversion utilities (in `votekit.pref_profile`):
  - `rank_profile_to_ballot_dict`, `score_profile_to_ballot_dict`
  - `rank_profile_to_ranking_dict`, `score_profile_to_scores_dict`
  - `convert_row_to_rank_ballot`
  - `convert_rank_profile_to_score_profile_via_score_vector` — turn a ranked profile into a scored one using a score vector (e.g. a [[Positional|Borda]]-style vector).

## How profiles are created

1. **By hand** — pass a list of [[Ballot|Ballots]].
2. **Generated** — from a [[Ballot Generators|ballot generator]] (synthetic electorates).
3. **Loaded** — from real data via [[CVR Loaders]].

## Example

```python
from votekit import PreferenceProfile, Ballot

profile = PreferenceProfile(
    ballots=[
        Ballot(ranking=[{"A"}, {"B"}, {"C"}], weight=10),
        Ballot(ranking=[{"B"}, {"A"}, {"C"}], weight=7),
    ]
)
```

## See also
- [[Ballot]] · [[Cleaning]] · [[Ballot Generators]] · [[CVR Loaders]]
- [[Preference Interval]] — the distribution generators sample profiles from.
