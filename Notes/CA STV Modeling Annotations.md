> [!PDF|important] [[CA_STV_Modeling.pdf#page=2&selection=34,0,36,23&color=important|Conventional wisdom hold that STV is "semi-proportional": while not providing guarantees, it is thought to produce more proportional outcomes than standard first-past-the-post (otherwise known as plurality) elections]]
> > Conventional wisdom hold that STV is "semi-proportional": while not providing guarantees, it is thought to produce more proportional outcomes than standard first-past-the-post (otherwise known as plurality) elections

> [!PDF|note] [[CA_STV_Modeling.pdf#page=2&selection=43,0,47,39&color=note|For instance, in a district that elects four members, just 20% of the vote is needed to secure a seat.]]
> > For instance, in a district that elects four members, just 20% of the vote is needed to secure a seat.

$Quota$ = $\frac{Valid Votes Cast}{Seats Available +1 }+1$

$20\%$ = $\frac{Valid Votes Cast}{4 + 1 }+1$

> [!PDF|yellow] [[CA_STV_Modeling.pdf#page=2&selection=51,0,71,49&color=yellow|STV can elect a variable number of winners by setting the district magnitude (or number of seats) to be any number m. Frequently, district magnitude of three to five is cited as an ideal size that balances the benefits of proportionality with the manageability of the election. So for example a nine-member city council would require nine districts using standard plurality voting, but could be configured in a 3 × 3 fashion (three districts electing three members each) or as a citywide at-large election in which voters choose all nine at once.]]
> STV can elect a variable number of winners by setting the district magnitude (or number of seats) to be any number m. Frequently, district magnitude of three to five is cited as an ideal size that balances the benefits of proportionality with the manageability of the election. So for example a nine-member city council would require nine districts using standard plurality voting, but could be configured in a 3 × 3 fashion (three districts electing three members each) or as a citywide at-large election in which voters choose all nine at once.

**What are the implications of choosing different district magnitudes in regards to representation, and how does instant-runoff compare for single member districts? Does STV result in better proportional representation than IRV, and does IRV result in better representation than plurality?**

**Is there an advantage to have more districts if you have more clustered identify groups with shared interests?**

> [!PDF|yellow] [[CA_STV_Modeling.pdf#page=12&selection=273,79,280,1&color=yellow]]
> > One of the eight votes was a surplus over threshold, so the ballots with C in first place will transfer with a "discount" factor of 1 8 .

**I don't quite understand the "discount" factor. What is this and how is it applied?** 

> A: It appears to represent the fraction of the winner's total votes that were surplus. In this example, the winner had 8 votes and 1 was surplus - so the discount factor would be 1/8. Indeed, the discount factor is a manner by which surplus votes for a winner are transferred to the next candidate on a given ballot.
> 
> Note that fractional transfer is a different mechanism than when a candidate is eliminated - in those cases, the next-preferred candidate on the ballot gets the full vote, rather than a fraction. Intuition says that if a voter's preferred candidate gets eliminated, we wouldn't want to dilute their vote by doing fractional transfer. Similarly, if they DO get their preferred candidate elected, we wouldn't want their vote to continue to "overpower" others by giving a full vote to their next preferred candidate. I suspect we'd end up with something looking similar to block plurality.

**What happens if you don't do fractional transfer, and you just give the next preferred candidate the full vote rather than a fraction in the case where a preferred candidate on a ballot gets elected?**

> [!PDF|yellow] [[CA_STV_Modeling.pdf#page=13&selection=51,46,57,6&color=yellow]]
> > One way to interpret the Droop quota is that it is the minimum number of votes such that no more than m candidates can achieve the quota.

What happens if you have 100 voters with 4 seats to be filled and five candidates, and in the first round every candidate receives 20% of the vote? In this example, the droop quota would be 21 - so nobody would win this round. But who gets eliminated? Do you just go right to counting second place preferences?

Different jurisdictions handle this differently:

- Random selection among the tied candidates
- Predetermined rule (e.g., alphabetical)
- Some systems examine 2nd preferences of the tied candidates to see if one has stronger downstream support

