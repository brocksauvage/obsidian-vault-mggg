

## Process Overview 

1. [[Ballot Generation|Generate]] realistic ranked-choice ballots for voters using [[Behavioral Ranking Models|ranking models]] 
2. Generate redistricting plans using a randomized [[Recombination Algorithm]].

#### Parameters

- **Cohesion**: the strength of the coin flip - or the chance that a voter from group A lists a candidate from slate A after slate B.
- **Turnout**: the rate at which each voting group votes
- **Candidate Strength**: how much a group coalesces around an individual candidate from one slate or another
### Generating Ballots

This step involves [[Ballot Generation|generating ballots]] that realistically represent voter preferences. 

