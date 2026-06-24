---
tags:
  - votekit
---

# Matrices

The **`votekit.matrices`** subpackage computes candidate-by-candidate matrices that summarize relationships across a [[Preference Profile]], plus a generic heatmap renderer.

## Functions

| Function | Computes |
|---|---|
| `comention` | Whether/how often two candidates appear together on ballots |
| `comention_above` | Co-mention where one candidate is ranked above the other |
| `comentions_matrix` | Full candidate × candidate co-mention matrix |
| `boost_prob` | Probability one candidate "boosts" another |
| `boost_matrix` | Full pairwise boost matrix |
| `candidate_distance` | Distance between two candidates (from ballot behavior) |
| `candidate_distance_matrix` | Full candidate × candidate distance matrix |
| `matrix_heatmap` | Render any of the above matrices as a heatmap |

## Why these exist

These matrices quantify **candidate relationships** — affinity, co-listing, and influence — which is central to MGGG's work on **slate/bloc voting and polarization**. They pair naturally with the [[Ballot Generators#2. Bloc / slate generators|slate generators]] (which build polarization in) and with [[Metrics]] (which measure it).

## Example

```python
from votekit.matrices import comentions_matrix, matrix_heatmap

M = comentions_matrix(profile)
matrix_heatmap(M)
```

## See also
- [[Graphs and Visualization]] · [[Metrics]] · [[Preference Profile]]
