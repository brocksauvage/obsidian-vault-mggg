---
tags: [gerrychain, proposals]
---

# Spectral Proposals

**Module:** `gerrychain/proposals/spectral_proposals.py`
**Part of:** [[GerryChain Architecture]] · [[Proposals]]

Spectral proposals split a region using **spectral graph theory** — the eigenvectors of the [[Graph|graph]] Laplacian — instead of a random spanning tree as [[ReCom]] does.

## `spectral_cut(subgraph, part_labels, weight_type, lap_type)`

Computes the **Fiedler vector** (the eigenvector of the second-smallest eigenvalue) of the subgraph's Laplacian and splits nodes by the sign of their entry in that vector. The Fiedler vector is the classic spectral-clustering cut: it tends to separate the graph into two well-connected pieces along its natural "bottleneck."

- `lap_type` selects the ordinary vs. **normalized** Laplacian (`Graph.laplacian_matrix()` / `normalized_laplacian_matrix()`).
- `weight_type` selects optional edge weighting.

## `spectral_recom(partition, ...)`

A [[ReCom]]-style proposal that, instead of cutting a random spanning tree, merges an adjacent district pair and re-splits the merged region with `spectral_cut`. Plug it into a [[MarkovChain]] like any other proposal.

> [!note] Trade-offs
> Spectral cuts can produce visually compact pieces but do not directly enforce population balance the way [[ReCom|tree-based ReCom]] does, so pair them carefully with population [[Bounds|constraints]].

## See also
- [[Graph]] — supplies the Laplacian matrices
- [[ReCom]], [[Proposals]]
