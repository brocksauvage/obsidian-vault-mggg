---
tags: [gerrychain, metrics, concept]
---

# Efficiency Gap

**Implemented in:** `gerrychain/metrics/partisan.py` → `efficiency_gap(election_results)`
**Part of:** [[Metrics]] · [[GerryChain Architecture]]

The **efficiency gap (EG)** is the most widely cited single-number measure of partisan gerrymandering. It quantifies the difference in **wasted votes** between two parties.

## Wasted votes

A vote is "wasted" if it did not contribute to winning a seat:

- **Lost votes:** all votes for the losing party in a district.
- **Surplus votes:** the winning party's votes *beyond* the 50% + 1 needed to win.

`wasted_votes(party1_votes, party2_votes)` computes these for a single district. Gerrymandering via **packing** (concentrating opponents into a few districts) and **cracking** (spreading them thin across many) both work by making the other party waste more votes.

## The metric

$$EG = \frac{W_A - W_B}{\text{total votes}}$$

where $W_A, W_B$ are the total wasted votes of each party. An EG near 0 is balanced; a large magnitude signals systematic advantage for one party. `efficiency_gap(election_results)` computes this from an [[Election|ElectionResults]] object.

## In the GerryChain workflow

Compute the efficiency gap of the enacted plan, then compute it for every plan in a neutral [[MarkovChain]] ensemble. If the enacted plan's EG sits in the extreme tail of the ensemble distribution, that is quantitative evidence the map is an outlier.

## See also
- [[Metrics]] — other partisan and compactness scores
- [[Election]] — supplies the vote data
- [[Polsby-Popper Score]] — the compactness analogue
