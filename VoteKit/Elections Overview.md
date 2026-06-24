---
tags:
  - votekit
aliases:
  - Election
  - ElectionState
---

# Elections Overview

The **`votekit.elections`** subpackage runs a voting rule on a [[Preference Profile]] and records the outcome. It is the analytic heart of VoteKit.

## The `Election` base class

`Election` (abstract, in `votekit.models`) is the parent of every voting rule. Key behavior:

- **Runs on construction.** When you instantiate an election with a profile, it immediately runs the whole rule to completion (`_run_election`).
- **Stores a history.** Results are kept as `election_states` — a list with **one [[#ElectionState|ElectionState]] per round** (index 0 = initial state, index 1 = round 1, …). `length` is the number of rounds.
- **Replayable.** `get_profile(round_number)` and `get_step(round_number)` reconstruct the profile/state at any round (negative indexing allowed).
- **Configurable scoring.** A `score_function` maps a profile → `{candidate: score}` for sorting candidates each round; `sort_high_low` chooses sort direction.

```python
from votekit.elections import STV

election = STV(profile, m=3)      # runs immediately
winners = election.get_elected()  # final winners
state = election.election_states[1]  # inspect round 1
```

## ElectionState

`ElectionState` (a `@dataclass` in `votekit.elections.election_state`) is the immutable record of **one round**. To save memory it does **not** carry the profile. Fields:

| Field | Meaning |
|---|---|
| `round_number` | Which round (0 = initial) |
| `remaining` | Still-in-the-running candidates, ordered, ties as frozensets |
| `elected` | Candidates elected this state |
| `eliminated` | Candidates eliminated this state |
| `tiebreaks` | How ties were resolved (`{tied set: resolution}`) |
| `scores` | Per-candidate scores for remaining candidates |

The ordered-tuple-of-frozensets pattern (same as a [[Ballot|ballot's]] ranking) lets a state express both an ordering and ties.

## The three families of rules

VoteKit groups concrete election types by the **ballot type** they consume:

- [[Ranking Elections]] — consume `RankBallot`s (`RankingElection` subclasses).
- [[Score Elections]] — consume `ScoreBallot`s.
- [[Approval Elections]] — approval-style score ballots.

## Transfers

For multi-winner ranked rules ([[Single Transferable Vote|STV]]), surplus is moved with **transfer functions** in `votekit.elections.transfers`:
- `fractional_transfer` — split surplus proportionally across ballots.
- `random_transfer` — move whole ballots chosen at random.

## See also
- [[Ranking Elections]] · [[Score Elections]] · [[Approval Elections]]
- [[Preference Profile]] — the input · [[Architecture Overview]]
