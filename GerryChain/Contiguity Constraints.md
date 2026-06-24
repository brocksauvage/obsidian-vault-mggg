---
tags: [gerrychain, constraints]
---

# Contiguity Constraints

**Module:** `gerrychain/constraints/contiguity.py`
**Part of:** [[GerryChain Architecture]] · [[Constraints]]

These constraints ensure that every district remains **connected** (contiguous) in the dual [[Graph]] — a near-universal legal requirement for districting plans. A district is contiguous if you can walk between any two of its units through adjacent units of the same district.

## The functions

| Function | Use |
|---|---|
| `contiguous(partition)` | Are **all** districts connected? The general-purpose check. |
| `single_flip_contiguous(partition)` | A fast check valid only when the proposal changed **one node** (used with single-flip [[Proposals]]). |
| `contiguous_bfs(partition)` | Contiguity via breadth-first search. |
| `number_of_contiguous_parts(partition)` | Count of connected pieces (useful as a diagnostic/score). |
| `contiguous_components(partition)` | The connected components of each district. |
| `is_connected_bfs(graph)` | Low-level BFS connectivity test on a [[Graph]]. |

## Why a special single-flip version?

Checking full contiguity after every step is expensive. When you know only one node moved (as in `propose_random_flip`), `single_flip_contiguous` does a localized check that is much faster than re-scanning every district — an example of GerryChain's general theme of **incremental computation**.

[[ReCom]] produces contiguous districts by construction (it splits a connected spanning tree), so it is often run without an explicit contiguity constraint, though one can still be added for safety.

## See also
- [[Constraints]], [[Validator]]
- [[Graph]] — where connectivity lives
- [[Proposals]] — single-flip proposals pair with `single_flip_contiguous`
