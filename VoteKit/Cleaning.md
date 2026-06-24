---
tags:
  - votekit
aliases:
  - Cleaning Functions
---

# Cleaning

The **`votekit.cleaning`** subpackage normalizes and repairs [[Ballot|ballots]] and [[Preference Profile|profiles]] — essential when working with real [[CVR Loaders|CVR data]]. Cleaning a profile returns a `CleanedRankProfile` / `CleanedScoreProfile` that records what changed.

- **Module:** `votekit.cleaning`

## Function families

Functions follow a consistent naming pattern: `<operation>_<rank|score>_<ballot|profile>`.

### Ranked profiles
- `clean_rank_profile` — apply a custom cleaning function across a profile.
- `remove_cand_rank_profile` — drop one or more candidates.
- `remove_repeat_cands_rank_profile` — remove duplicate candidates within ballots.
- `condense_rank_profile` — close gaps left by removed/empty rank positions.
- `remove_and_condense_rank_profile` — remove candidates **and** condense in one step.

### Ranked ballots (single-ballot versions)
- `remove_cand_rank_ballot`
- `remove_repeat_cands_rank_ballot`
- `condense_rank_ballot`

### Scored profiles / ballots
- `clean_score_profile`
- `remove_cand_score_profile`
- `remove_cand_score_ballot`

## Why it matters

- **Elimination rounds** in rules like [[Single Transferable Vote|STV]]/[[Ranked Choice Voting|IRV]] rely on *condensing* ballots after a candidate is removed.
- Real ballots have **skips, repeats, and overvotes** that must be resolved before a rule can be applied consistently.

## Example

```python
from votekit.cleaning import remove_and_condense_rank_profile

clean = remove_and_condense_rank_profile(profile, ["write-in"])
```

## See also
- [[Preference Profile]] · [[Ballot]] · [[CVR Loaders]]
