---
tags: [gerrychain, proposals, moc]
---

# Proposals

**Module:** `gerrychain/proposals/`
**Part of:** [[GerryChain Architecture]]

A **proposal** is the `proposal` argument of a [[MarkovChain]]: a function with signature `Partition -> Partition` that suggests the next state. The choice of proposal defines *how the chain explores the space of districting plans*.

There are two broad families:

## 1. Flip-based proposals (`proposals/proposals.py`)

These move one or a few nodes between districts. They make small, local moves and are simple but mix slowly.

| Function | Move |
|---|---|
| `propose_random_flip` | Flip a single boundary node to a neighboring district. |
| `propose_chunk_flip` | Flip a connected chunk of boundary nodes at once. |
| `propose_any_node_flip` | Flip any node (not restricted to one boundary). |
| `propose_flip_every_district` | Propose one flip per district per step. |
| `slow_reversible_propose` / `slow_reversible_propose_bi` | Reversible single-flip proposals (well-defined transition probabilities, needed for theory but slow). |

All of these produce the next state via [[Partition|partition.flip()]].

## 2. Tree-based proposals — [[ReCom]]

[[ReCom]] (ReCombination) merges two adjacent districts and re-splits them along a random spanning tree. It makes **large, population-balanced** moves and mixes far faster than flips, which is why it is the de facto standard proposal. See [[ReCom]] and [[Tree Partitioning]].

## 3. [[Spectral Proposals]]

`spectral_recom` / `spectral_cut` split a region using the [[Graph|graph]] Laplacian's Fiedler vector instead of a random spanning tree.

## Choosing a proposal

- Use [[ReCom]] for ensemble generation and most real analyses.
- Use flip proposals when you need a single-step, theoretically reversible chain or are studying the [[Metagraph]].

## See also
- [[ReCom]], [[Spectral Proposals]], [[Tree Partitioning]]
- [[MarkovChain]] — consumes a proposal
- [[Acceptance Functions]] — paired with proposals to define the chain dynamics
