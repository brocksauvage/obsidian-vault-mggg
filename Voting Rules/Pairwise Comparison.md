---
tags:
  - voting-rule
aliases:
  - Condorcet Method
  - Pairwise Voting
---

# Pairwise Comparison

**Pairwise comparison** (the **Condorcet** family of methods) uses ranked ballots to simulate head-to-head contests between every pair of candidates. A candidate who beats every other candidate one-on-one is the **Condorcet winner**.

## How it works
- Voters rank candidates.
- For each pair of candidates, count how many ballots prefer one over the other.
- The candidate who wins all pairwise matchups wins the election.

## Key characteristics
- Identifies the candidate a majority prefers in every direct comparison.
- A Condorcet winner does not always exist—pairwise results can cycle (the *Condorcet paradox*); completion methods (e.g., Schulze, Ranked Pairs) resolve this.
- Considered theoretically strong but more complex to compute and explain.

## Related
- [[Ranked Choice Voting]] and [[Positional]] — other ranked-ballot rules that may pick a different winner.
