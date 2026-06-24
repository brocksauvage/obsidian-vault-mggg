---
tags: [gerrychain, architecture, moc]
---

# GerryChain Architecture

> [!info] What is GerryChain?
> **GerryChain** is a Python library, developed and maintained by the [[Data and Democracy Lab|Metric Geometry and Gerrymandering Group (MGGG)]], for building **ensembles of districting plans** using [Markov chain Monte Carlo](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) (MCMC). It is distributed under the 3-Clause BSD License.

This folder documents how the GerryChain codebase is architected, with one note per major Python class. This page is the **map of content (MOC)** — start here.

> [!tip] Visual class diagram
> See [[GerryChain Class Diagram.canvas|GerryChain Class Diagram]] for an interactive Obsidian Canvas of these classes and their relationships (inheritance, composition, and usage). Every box opens the corresponding class note.

## The Core Idea

The basic workflow is:

1. Start with the **geometry of an initial districting plan** (a [[Graph]] of geographic units plus an [[Assignment]] of units to districts).
2. Generate a **large ensemble of alternative plans** by running a [[MarkovChain]]. Each step proposes a small modification, checks it against [[Constraints]], and accepts or rejects it.
3. **Compare** the initial plan to the ensemble. If the initial plan is an *outlier* against thousands of neutrally-drawn plans, that is quantitative evidence of gerrymandering.

```python
from gerrychain import MarkovChain, Partition, Graph

chain = MarkovChain(proposal, constraints, accept, initial_state, total_steps)
for partition in chain:
    # compute scores, record statistics, ...
```

## The Five Pillars

A [[MarkovChain]] is constructed from five ingredients. Almost every class in the library exists to support one of them:

| Ingredient | Role | Key classes / notes |
|---|---|---|
| `initial_state` | The starting plan (a state of the chain) | [[Partition]], [[GeographicPartition]], [[Grid]] |
| `proposal` | Proposes the next state | [[ReCom]], [[Proposals]], [[Spectral Proposals]] |
| `constraints` | Decides whether a proposed state is *valid* | [[Validator]], [[Bounds]], [[Contiguity Constraints]] |
| `accept` | Decides whether to *move* to a valid state | [[Acceptance Functions]] |
| `total_steps` | How many iterations to run | (integer) |

## Layered Component Map

```
                          ┌──────────────────┐
                          │   MarkovChain    │  iterates states
                          └────────┬─────────┘
            proposal │   constraints │   accept │
        ┌────────────┴──┐  ┌─────────┴────┐  ┌──┴────────────┐
        │  Proposals /  │  │  Validator / │  │  Acceptance   │
        │    ReCom      │  │  Bounds      │  │  Functions    │
        └───────┬───────┘  └──────────────┘  └───────────────┘
                │ operate on
        ┌───────┴────────────────────────────────────┐
        │              Partition (a state)            │
        │  • Assignment   (nodes → districts)         │
        │  • SubgraphView (districts → subgraphs)     │
        │  • Updaters     (cached derived statistics) │
        └───────┬─────────────────────────────────────┘
                │ wraps
        ┌───────┴───────┐
        │ Graph /       │  the dual graph of geographic units
        │ FrozenGraph   │
        └───────────────┘
```

## Notes in this folder

### Core data model
- [[Graph]] — the dual graph of geographic units (the foundation of everything)
- [[FrozenGraph]] — an immutable, cached wrapper used inside the chain for speed
- [[Partition]] — a *state* of the Markov chain: a graph plus a district assignment
- [[GeographicPartition]] — a `Partition` that also tracks areas, perimeters, and boundaries
- [[Assignment]] — the node → district mapping with cheap part lookups
- [[SubgraphView]] — lazily-computed, cached subgraph per district
- [[Grid]] — a synthetic grid `Partition` for testing and demos

### Moving through the state space
- [[Proposals]] — flip-based and overview of proposal functions
- [[ReCom]] — ReCombination, the workhorse spanning-tree proposal
- [[Spectral Proposals]] — proposals based on the graph Laplacian
- [[Tree Partitioning]] — spanning-tree machinery (`bipartition_tree`, `recursive_tree_part`, [[PopulatedGraph]])
- [[Acceptance Functions]] — `always_accept`, `cut_edge_accept`

### Validity
- [[Constraints]] — overview of the constraint system
- [[Validator]] — bundles constraints into a single yes/no predicate
- [[Bounds]] — numeric bound constraints (population, compactness, …)
- [[Contiguity Constraints]] — keep districts connected

### Derived statistics (Updaters)
- [[Updaters]] — overview of the updater pattern
- [[Tally]] — sum node attributes per district (and [[DataTally]])
- [[Election]] — model elections per plan (and ElectionResults)

### Analysis & optimization
- [[SingleMetricOptimizer]] — short bursts, simulated annealing, tilted runs
- [[Gingleator]] — optimize for majority-minority (Gingles) districts
- [[Metrics]] — partisan metrics ([[Efficiency Gap]], mean-median, …) and [[Polsby-Popper Score|compactness]]
- [[Diversity Stats]] — measure how diverse an ensemble is
- [[Metagraph]] — reason about the graph of all reachable states

## Related concepts
- [[Gerry Chain]] — learning roadmap
- [[Electoral District]]
- [[Polsby-Popper Score]]
