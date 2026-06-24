---
tags:
  - votekit
  - moc
aliases:
  - VoteKit Home
---

# VoteKit

> [!info] Reference
> A Python package for computational social choice research, developed by [MGGG](https://mggg.org/).
> - **Source:** https://github.com/mggg/VoteKit
> - **Docs:** https://votekit.readthedocs.io/en/latest/
> - **Install:** `pip install votekit`
> - **Documented version:** v3.4.0

`VoteKit` is "a Swiss army knife for computational social choice research." It provides the tools to **represent** ballots and electorates, **generate** synthetic voting data or **load** real cast-vote records, **run** elections under many voting rules, and **analyze/visualize** the results.

## Map of Content

### Core data structures
- [[Ballot]] — the atomic unit: a ranking or a score vector with a weight.
- [[Preference Profile]] — a collection of ballots representing an electorate.
- [[Preference Interval]] — candidate support distribution used by generators.

### Getting data in
- [[Ballot Generators]] — synthetic electorates (IC, IAC, spatial, slate/bloc models).
- [[CVR Loaders]] — read real-world cast-vote-record files.
- [[Cleaning]] — normalize and repair ballots and profiles.

### Running elections
- [[Elections Overview]] — the `Election` / `ElectionState` workflow.
- [[Ranking Elections]] — rules that consume ranked ballots.
- [[Score Elections]] — rules that consume scored ballots.
- [[Approval Elections]] — approval and block-approval rules.

### Analysis & visualization
- [[Graphs and Visualization]] — ballot graphs, pairwise graphs, plots, MDS.
- [[Matrices]] — boost, candidate-distance, and comention matrices + heatmaps.
- [[Metrics]] — distances between profiles/distributions.

## See also
- [[Architecture Overview]] — how the pieces fit together end-to-end.
- The lab's general voting-rule theory notes in the `Voting Rules/` folder — e.g. [[Plurality]], [[Ranked Choice Voting]], [[Single Transferable Vote]], [[Cumulative]], [[Pairwise Comparison]] (independent of VoteKit).
