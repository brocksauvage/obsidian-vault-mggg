
A mathematical measure of how compact an [[Electoral District|electoral district]] is. The formula is:

$$PP = \frac{4 \pi A}{P^2}$$ [1](https://ideas.repec.org/a/inm/oropre/v73y2025i5p2330-2350.html)

where A is the area of a district, and P is the perimeter. A $PP$ of 1 means a perfectly round district - the most compact. A score of 0 is a convoluted, jagged, or sprawling district.

### Aggregation and Distributions

When assessing multiple districts within a redistricting plan/partition, we may be interested in looking at the distribution of Polsby-Popper scores to see what proportion of districts are compact - or not. 
#### Weaknesses

Although a popular metric for flagging gerrymandering, it is sensitive to natural geography. If a district happens to be drawn up against a shoreline, mountain range, or intricate river border, the Polsby-Popper will probably be very low and not actually representative of compactness. Consider using **Reock Score** or **Convex Hull** in these cases.

### In GerryChain

GerryChain implements this as `polsby_popper` / `compute_polsby_popper` (see [[Metrics]]), computed from the area and perimeter updaters on a [[GeographicPartition]]. It can also be wrapped in [[Bounds]] to act as a compactness [[Constraints|constraint]] during a [[MarkovChain]] run.