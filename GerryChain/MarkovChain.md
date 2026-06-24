---
tags: [gerrychain, class, core]
---

# MarkovChain

**Module:** `gerrychain/chain.py`
**Part of:** [[GerryChain Architecture]]

`MarkovChain` is the central driver of the library. It is an **iterator** that yields successive [[Partition]] states of a Markov chain run, where each state is a candidate districting plan.

## Construction

```python
chain = MarkovChain(proposal, constraints, accept, initial_state, total_steps)
```

| Parameter | Type | Role |
|---|---|---|
| `proposal` | `Callable` | Given the current [[Partition]], propose the next one. See [[Proposals]] / [[ReCom]]. |
| `constraints` | `Validator` / iterable of callables | Is the proposed state *valid*? See [[Validator]], [[Bounds]]. |
| `accept` | `Callable` | Given a valid proposal, should we *move* there? See [[Acceptance Functions]]. |
| `initial_state` | [[Partition]] | The starting plan. **Must itself satisfy the constraints** or the constructor raises `ValueError`. |
| `total_steps` | `int` | Number of states to yield. |

If `constraints` is a bare callable it is automatically wrapped in a [[Validator]]. The constructor validates `initial_state` immediately and reports exactly which constraints failed.

## How iteration works

The class implements the Python iterator protocol:

- `__iter__` resets the counter to 0 and `state` to `initial_state`.
- `__next__` performs the **propose → validate → accept** loop:
  1. Call `proposal(state)` to get a proposed next state.
  2. Detach `state.parent` to avoid a memory leak (long chains would otherwise retain the whole history).
  3. If [[Validator|is_valid]] rejects the proposal, *re-propose* without advancing the counter.
  4. If valid, call `accept`. If accepted, move to the proposed state; either way, advance the counter and yield.
- After `total_steps`, raises `StopIteration`.

> [!note] Rejected vs. not-accepted
> An **invalid** proposal (fails constraints) does not consume a step — the chain keeps re-proposing. A **valid but not-accepted** proposal *does* consume a step; the chain simply yields the current (unchanged) state again. This is the standard Metropolis–Hastings rejection behavior.

## The `constraints` property

`constraints` is a read/write alias for the internal `is_valid` [[Validator]]. Re-assigning it re-checks the initial state against the new constraints and raises `ValueError` (listing failed constraints) if it no longer qualifies.

## Helpers

- `with_progress_bar()` — wraps the chain in a `tqdm` progress bar for long runs.
- `__len__` returns `total_steps`; `__repr__` → `<MarkovChain [N steps]>`.

## Typical usage

```python
for partition in chain.with_progress_bar():
    record(partition["my_election"].efficiency_gap())
```

## See also
- [[Partition]] — the state type yielded each step
- [[ReCom]], [[Proposals]] — proposal functions
- [[Validator]], [[Constraints]] — validity checking
- [[Acceptance Functions]] — the `accept` argument
- [[SingleMetricOptimizer]] — wraps this machinery for optimization rather than ensemble generation
