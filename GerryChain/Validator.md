---
tags: [gerrychain, class, constraints]
---

# Validator

**Module:** `gerrychain/constraints/validity.py`
**Part of:** [[GerryChain Architecture]] · [[Constraints]]

A `Validator` bundles a list of constraint functions into a **single callable predicate**. It is the object the [[MarkovChain]] uses to decide whether a proposed [[Partition]] is a legal state.

## Behavior

```python
is_valid = Validator([contiguous, pop_constraint, compactness_bound])
is_valid(partition)   # -> True only if EVERY constraint returns True
```

- `__init__(constraints)` stores the list of `Partition -> bool` callables.
- `__call__(partition)` returns `True` iff **all** constraints pass. It short-circuits / can report which constraint failed (the [[MarkovChain]] constructor uses `validator.constraints` to list failures by `__name__`).

Each constraint exposes a `__name__`, which is why the [[Bounds]] classes implement a `__name__` property — so validation failures produce readable error messages.

## Where it is used

- [[MarkovChain]] wraps its `constraints` argument in a `Validator` (if not already one) and rejects an `initial_state` that fails.
- [[SingleMetricOptimizer]] and [[Gingleator]] accept the same constraint forms.

## See also
- [[Constraints]] — the catalog of available constraints
- [[Bounds]] — numeric constraints that carry a `__name__`
- [[MarkovChain]]
