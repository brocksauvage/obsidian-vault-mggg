---
tags:
  - votekit
aliases:
  - VoteKit Architecture
---

# Architecture Overview

VoteKit is structured as a pipeline. Data flows from **ballots → profiles → elections → analysis**, with helper layers for generating and cleaning data along the way.

```
            ┌─────────────────────────────────────────────┐
            │              DATA SOURCES                     │
            │  Ballot Generators   |   CVR Loaders          │
            │  (synthetic)         |   (real CSV / Scottish)│
            └───────────────┬─────────────────┬────────────┘
                            │                  │
                            ▼                  ▼
                     ┌──────────────────────────────┐
   Cleaning  ◀────▶  │     PreferenceProfile         │
   (normalize)       │  RankProfile | ScoreProfile   │
                     │  built from Ballots           │
                     └───────────────┬──────────────┘
                                     │
                                     ▼
                     ┌──────────────────────────────┐
                     │         Election              │
                     │  runs rounds, produces a list │
                     │  of ElectionState objects     │
                     └───────────────┬──────────────┘
                                     │
                                     ▼
                     ┌──────────────────────────────┐
                     │   Analysis & Visualization    │
                     │  Graphs · Plots · Matrices ·  │
                     │  Metrics · Representation      │
                     └──────────────────────────────┘
```

## The layers

| Layer | Module(s) | Purpose |
|---|---|---|
| Core types | `votekit` (top level), `votekit.pref_profile`, `votekit.pref_interval` | [[Ballot]], [[Preference Profile]], [[Preference Interval]] |
| Generation | `votekit.ballot_generator` | Synthetic [[Ballot Generators\|profiles]] |
| Ingest | `votekit.cvr_loaders` | [[CVR Loaders\|Real data]] |
| Cleaning | `votekit.cleaning` | [[Cleaning\|Repair/normalize]] ballots & profiles |
| Elections | `votekit.elections` | [[Elections Overview\|Run rules]], `Election` + `ElectionState` |
| Graphs/Viz | `votekit.graphs`, `votekit.plots` | [[Graphs and Visualization]] |
| Matrices | `votekit.matrices` | [[Matrices]] |
| Metrics | `votekit.metrics`, `votekit.representation_scores` | [[Metrics]] |

## Key design ideas

- **Two ballot flavors throughout.** Almost every layer comes in a *ranking* variant and a *score* variant: `RankBallot`/`ScoreBallot`, `RankProfile`/`ScoreProfile`, ranking vs. score elections, ranking vs. score cleaning. This dichotomy is the backbone of the whole API.
- **Immutable, hashable data.** `Ballot` uses `__slots__` and is frozen after construction; identical ballots are deduplicated and carried with a `weight`. This keeps large profiles compact.
- **Elections are replayable histories.** An `Election` runs to completion on construction and stores one [[Elections Overview#ElectionState|ElectionState]] per round, so you can inspect any round after the fact.
- **Public import paths are curated.** Each subpackage `__init__` re-exports its public names and patches `__module__` so the documented path (e.g. `votekit.elections.STV`) matches the import path, hiding the deep internal layout.

## See also
- [[VoteKit]] — top-level index.
