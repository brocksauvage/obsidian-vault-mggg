---
tags: [gerrychain, constraints, moc]
---

# Constraints

**Module:** `gerrychain/constraints/`
**Part of:** [[GerryChain Architecture]]

**Constraints** define which proposed [[Partition|partitions]] are *valid* states of a [[MarkovChain]]. A constraint is any `Partition -> bool` predicate; the chain re-proposes whenever one fails. Constraints are how you keep an ensemble within "traditional districting principles" — balanced population, contiguous districts, reasonable compactness.

## How constraints are applied

Multiple constraints are bundled into a single [[Validator]], which returns `True` only if **all** of them pass. The [[MarkovChain]] (and [[SingleMetricOptimizer]]) accept either a [[Validator]] or a plain list of callables (auto-wrapped).

```python
from gerrychain.constraints import Validator, contiguous, within_percent_of_ideal_population

is_valid = Validator([
    contiguous,
    within_percent_of_ideal_population(initial_partition, 0.02),
])
```

## Families of constraints

| Family | Module | Notes |
|---|---|---|
| Numeric bounds | `constraints/bounds.py` | [[Bounds]], `UpperBound`, `LowerBound`, self-configuring variants. |
| Contiguity | `constraints/contiguity.py` | [[Contiguity Constraints]] — `contiguous`, `single_flip_contiguous`, … |
| Population validity | `constraints/validity.py` | `within_percent_of_ideal_population`, `deviation_from_ideal`, `districts_within_tolerance`, `no_vanishing_districts`, `refuse_new_splits`. |
| Compactness | `constraints/compactness.py` | L1/L2 [[Polsby-Popper Score|Polsby-Popper]] bounds (`L1_reciprocal_polsby_popper`, …). |

### Population validity (`validity.py`)
- `within_percent_of_ideal_population(partition, percent)` — keep every district within `percent` of the ideal. The most common population constraint.
- `deviation_from_ideal(partition, attribute="population")` — per-district fractional deviation (a diagnostic).
- `no_vanishing_districts(partition)` — forbid empty districts.
- `refuse_new_splits(field)` — forbid creating new county/region splits.

## See also
- [[Validator]] — bundles constraints into one predicate
- [[Bounds]] — numeric bound helpers
- [[Contiguity Constraints]] — connectivity checks
- [[MarkovChain]] — the `constraints` argument
- [[Polsby-Popper Score]] — basis of the compactness bounds
