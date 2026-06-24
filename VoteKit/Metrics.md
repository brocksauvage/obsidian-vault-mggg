---
tags:
  - votekit
aliases:
  - Distances
  - Representation Scores
---

# Metrics

VoteKit provides quantitative measures over profiles and distributions in **`votekit.metrics`** (distances) and **`votekit.representation_scores`** (representation).

## Distances (`votekit.metrics`)

| Function | Measures |
|---|---|
| `earth_mover_dist` | Earth Mover's / Wasserstein distance between two distributions |
| `lp_dist` | General L^p distance |
| `euclidean_dist` | Euclidean (L²) distance |

These compare two [[Preference Profile|profiles]] or [[Preference Interval|preference intervals]] — e.g. how far a [[Ballot Generators|generated]] electorate is from a target, or how different two blocs are. They underpin **metric / distance-based voting** analysis.

## Representation scores (`votekit.representation_scores`)

The `representation_score` module measures how well an outcome (a winner set) represents the electorate — used to evaluate fairness/proportionality of [[Elections Overview|election]] results.

## Example

```python
from votekit.metrics import earth_mover_dist

d = earth_mover_dist(profile_a, profile_b)
```

## See also
- [[Matrices]] · [[Graphs and Visualization]]
- [[Preference Profile]] · [[Preference Interval]]
