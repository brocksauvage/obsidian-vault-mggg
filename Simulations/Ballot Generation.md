The process of creating sets of voter ballots in voting simulations. This utilizes voter [[Preference Profile|preference profiles]] to generate a ballot that reflects the profile of individual voters. In aggregate, generated ballots can be used to simulate different electoral systems and observe expected results.

### Process

1. Voter selects a [[CA_STV_Modeling.pdf#page=6&selection=49,21,56,2|ballot type]] (e.g. AAABABBAAA)
	1. The impulsive voter fills in choices within each slate. Think of this as the voter flipping a weighted coin and choosing A or B based on the result.
	2. Deliberative voters will consider all pairwise comparisons between candidates, sorting them in an order aligned to how they are all "weighted."
	3. The Cambridge voter will rank candidates based on historical data, and unlike the other two, they will do so by using short ballots instead of ranking everyone.

