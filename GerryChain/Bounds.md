---
tags: [gerrychain, class, constraints]
---

# Bounds

**Module:** `gerrychain/constraints/bounds.py`
**Part of:** [[GerryChain Architecture]] · [[Constraints]]

The `bounds.py` module turns any numeric score function (a `Partition -> number`, or `-> iterable of numbers`) into a **constraint** by checking it against a threshold. These are the building blocks for population- and compactness-balance constraints used by the [[Validator]].

## The classes

| Class | Valid when… |
|---|---|
| `Bounds(func, bounds=(lo, hi))` | every value of `func(partition)` falls in `[lo, hi]`. |
| `UpperBound(func, bound)` | `func(partition) <= bound`. |
| `LowerBound(func, bound)` | `func(partition) >= bound`. |
| `SelfConfiguringUpperBound(func)` | upper bound auto-set from the **initial** partition's value (proposals may not get *worse*). |
| `SelfConfiguringLowerBound(func, epsilon=0.05)` | lower bound auto-set from the initial value, within `epsilon`. |
| `WithinPercentRangeOfBounds(func, percent)` | value stays within `percent` of the initial value. |

Each is callable (`bound(partition) -> bool`) and each implements a `__name__` property so the [[Validator]] can name it in failure messages.

## The "self-configuring" pattern

Self-configuring bounds read the initial [[Partition]]'s score and set the threshold from it. This encodes the common GerryChain goal: *"the ensemble should perform at least as well as the starting plan."* For example, wrapping a compactness score in a `SelfConfiguringLowerBound` forbids proposals that make the plan markedly less compact than where you started.

```python
from gerrychain.constraints import SelfConfiguringLowerBound
from gerrychain.constraints.compactness import L1_reciprocal_polsby_popper

compactness_bound = SelfConfiguringLowerBound(L1_reciprocal_polsby_popper)
```

## See also
- [[Constraints]], [[Validator]]
- [[Polsby-Popper Score]] — common score wrapped by these bounds
- [[Metrics]]
