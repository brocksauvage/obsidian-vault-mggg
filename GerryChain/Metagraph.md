---
tags: [gerrychain, metagraph]
---

# Metagraph

**Module:** `gerrychain/metagraph.py`
**Part of:** [[GerryChain Architecture]]

The **metagraph** is the graph whose *nodes are entire districting plans* and whose *edges connect plans that differ by a single valid move*. While a [[Partition]] is one state, the metagraph is the whole **state space** the [[MarkovChain]] walks. This module provides tools to enumerate and measure the local neighborhood of a state.

## Functions

| Function | Returns |
|---|---|
| `all_cut_edge_flips(partition)` | Every single-node flip available along the current cut edges (each as a `flips` dict). |
| `all_valid_states_one_flip_away(partition, constraints)` | Every neighboring [[Partition]] reachable by one flip that satisfies the [[Constraints|constraints]]. |
| `all_valid_flips(partition, constraints)` | The valid flip dicts (rather than the resulting partitions). |
| `metagraph_degree(partition, constraints)` | The **degree** of the current state in the metagraph — how many valid plans are one flip away. |

## Why it matters

The metagraph degree is central to **reversibility and correct sampling**. To know the probability of proposing a given move (needed for Metropolis–Hastings [[Acceptance Functions|acceptance]] and for the reversible variants of [[ReCom|ReCom]]), you must know how many moves are available from each state. These helpers compute that local structure.

## See also
- [[Partition]], [[MarkovChain]]
- [[Constraints]] — used to filter valid neighbors
- [[Proposals]] — single-flip proposals operate on this neighborhood
