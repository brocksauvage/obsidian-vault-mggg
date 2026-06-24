---
tags: [gerrychain, class, trees]
---

# PopulatedGraph

**Module:** `gerrychain/tree.py`
**Part of:** [[GerryChain Architecture]] · [[Tree Partitioning]]

`PopulatedGraph` wraps a (spanning-tree) [[Graph]] together with **population data per node**, providing the operations needed while searching for a balanced edge cut.

## Role

When [[Tree Partitioning|bipartition_tree]] looks for an edge whose removal yields a piece of the target population, it works on a `PopulatedGraph`. The class tracks each node's accumulated population as the tree is contracted toward the root.

## Key members

| Member | Purpose |
|---|---|
| `degree(node)` | Degree of a node in the (shrinking) tree. |
| `contract_node(node, parent)` | Collapse a leaf into its parent, accumulating population — the core step of the contraction cut search. |
| `has_ideal_population(node, one_sided_cut=False)` | Is the subtree rooted here within tolerance of the target population? (i.e. is the edge above it a valid balanced cut?) |
| `__iter__`, `__repr__` | Iterate nodes / display. |

## See also
- [[Tree Partitioning]] — the algorithms that drive it
- [[ReCom]] — ultimate consumer
