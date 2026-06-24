---
tags:
  - votekit
aliases:
  - Plots
  - Graphs
  - Visualization
---

# Graphs and Visualization

VoteKit ships two related visual/analytic layers: **graph structures** (`votekit.graphs`) and **plotting functions** (`votekit.plots`).

## Graphs (`votekit.graphs`)

| Object | Purpose |
|---|---|
| `BallotGraph` | Graph whose nodes are ballots/rankings, with edges between "adjacent" ballots — used to study the structure of a [[Preference Profile]]'s ballot space |
| `PairwiseComparisonGraph` | Directed graph of head-to-head margins between candidates — the structure behind [[Pairwise Comparison]]/Condorcet analysis |
| `pairwise_dict` | Build the dict of pairwise margins from a profile |
| `restrict_pairwise_dict_to_subset` | Restrict pairwise results to a candidate subset |

`PairwiseComparisonGraph` makes Condorcet winners/cycles visible — directly relevant to [[Ranking Elections#Condorcet / pairwise family|RankedPairs and Schulze]].

## Plots (`votekit.plots`)

General bar plots:
- `bar_plot`, `multi_bar_plot`

Profile-specific plots (and `multi_profile_*` variants for comparing profiles):
| Function | Shows |
|---|---|
| `profile_bar_plot` | Generic profile bar chart |
| `profile_fpv_plot` | First-place votes per candidate |
| `profile_borda_plot` | [[Positional\|Borda]] scores |
| `profile_ballot_lengths_plot` | Distribution of ballot lengths |
| `profile_mentions_plot` | How often each candidate is mentioned |

Dimensionality reduction:
- `compute_MDS` / `plot_MDS` — multidimensional scaling to embed/visualize ballots or candidates in 2-D.

## Example

```python
from votekit.graphs import PairwiseComparisonGraph
from votekit.plots import profile_fpv_plot

pcg = PairwiseComparisonGraph(profile)
profile_fpv_plot(profile)
```

## See also
- [[Matrices]] — numeric companions (heatmaps) to these visuals.
- [[Metrics]] · [[Preference Profile]]
