---
tags: [gerrychain, class, core]
---

# Assignment

**Module:** `gerrychain/partition/assignment.py`
**Part of:** [[GerryChain Architecture]]

`Assignment` maps the nodes of a [[Graph]] to **parts** (districts). It subclasses `collections.abc.Mapping`, so it behaves like a read-only dictionary `node → district`, while *also* keeping a reverse index so that "which nodes are in this district?" is cheap.

## Dual representation

| Attribute | Form | Answers |
|---|---|---|
| `mapping` | `{node: part}` | "Which district is this node in?" |
| `parts` | `{part: frozenset(nodes)}` | "Which nodes are in this district?" |

Keeping both in sync is what lets a [[Partition]] and its proposals/updaters pick whichever direction is faster. The `parts` values are `frozenset`s (validated on construction).

## Construction & validation

```python
Assignment(parts, mapping=None, validate=True)
```
With `validate=True` it checks that every node has a **unique** assignment (raises `ValueError` otherwise) and that the part values are `frozenset`s (`TypeError` otherwise). If `mapping` is omitted it is derived from `parts`.

Helper `get_assignment(...)` (same module) builds an `Assignment` from a dict, a `pandas.Series`, or a node-attribute name on the graph — this is what [[Partition]] uses to accept flexible `assignment` arguments.

## Incremental updates

`update_flows(flows)` mutates the assignment for just the nodes that changed between consecutive states (the *flows*). This is central to how [[Partition|Partition.flip()]] produces a new state cheaply rather than rebuilding the whole mapping. `copy()` duplicates the `parts`/`mapping` dicts (but shares the underlying frozensets).

## See also
- [[Partition]] — owns an `Assignment` and updates it via flows
- [[Graph]] — supplies the node set
- [[SubgraphView]] — uses `parts` to build per-district subgraphs
