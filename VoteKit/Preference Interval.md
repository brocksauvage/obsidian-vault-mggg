---
tags:
  - votekit
  - core-type
aliases:
  - PreferenceInterval
---

# Preference Interval

A **`PreferenceInterval`** encodes how much support a set of candidates has, as a probability distribution that sums to 1 — think of the unit interval cut into segments, one per candidate, sized by support.

- **Module:** `votekit.pref_interval`
- **Public class:** `PreferenceInterval` (importable from `votekit`)

## Role in the package

Preference intervals are the **input parameters for many [[Ballot Generators|ballot generators]]**. Sampling models such as Plackett–Luce and Bradley–Terry draw rankings by repeatedly selecting candidates in proportion to their share of the interval, so the interval defines the "true" relative appeal of candidates that the generator then turns into noisy ballots.

In bloc/slate models, each bloc of voters can have its **own** preference interval over the slates of candidates, which (together with cohesion parameters) drives polarized voting behavior.

## Example

```python
from votekit import PreferenceInterval

# A is twice as appealing as B, which is twice as appealing as C
pi = PreferenceInterval({"A": 0.5, "B": 0.25, "C": 0.25})
```

## See also
- [[Ballot Generators]] — consume preference intervals to produce [[Preference Profile|profiles]].
- [[VoteKit]]
