# Structure

- Intro what's proof of stake and why addapt it
- Difference with pow (side-by-side)
- Casper essentials
  - Two slashing conditions
  - Correct-by-construction fork choice rule, use the longest justified chain, justified means that has 2/3 of the votes.
  - Dynamic validator sets


# NOTES

Ethereum Proof of Stake Essentials: An Overview of Casper

From [2]

- Two slashing conditions

- a correct-by-construction fork choice rule, use the longest justified chain, justified means that has 2/3 of the votes.

- dynamic validator sets

Casper It's a BFT (Byzantine fault tolerant)[1]

The two Casper Commandments

AN INDIVIDUAL VALIDATOR ν MUST NOT PUBLISH TWO DISTINCT VOTES, SUCH THAT EITHER

a validator must not publish two distinct votes for the same target height
Equivalently, a validator must not vote within the span of its other votes


[1] https://medium.com/loom-network/understanding-blockchain-fundamentals-part-1-byzantine-fault-tolerance-245f46fe8419

[2] https://arxiv.org/pdf/1710.09437.pdf
