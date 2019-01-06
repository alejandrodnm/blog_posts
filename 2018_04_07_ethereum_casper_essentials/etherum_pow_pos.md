![casper](casper.jpg)
# Ethereum's Casper essentials: Hybrid Proof of Work & Proof of Stake system

For several years, the Ethereum community has been working hard into the design 
and development of Casper, a system that will implement a proof of stake (PoS)
algorithm on the blockchain, with the purpose of gradually moving away from
proof of work (PoW).

Casper the Friendly Finality Gadget (FFG), as it is called in its [whitepaper](https://arxiv.org/pdf/1710.09437.pdf),
is its first iteration. Taking the form of a hybrid PoW/PoS system, it overlays
the PoS protocol on top of the current one that uses PoW. Casper is planned to
come out with Ethereum's next hard fork titled Constantinople, although there is
no official release date, rumors point to the last quarter of 2018.

There is currently a [tesnet](http://34.203.42.208:3000/) running Casper, it
launched the last day of 2017 if you want to play around with it, be sure to check
[Alpha Casper FFG Testnet Instructions](https://hackmd.io/s/Hk6UiFU7z#) and
[Casper Pyethapp Development Environment Containers](https://github.com/karlfloersch/docker-pyeth-dev).

## Proof of Stake vs Proof of Work

Proof of work, in simple terms, consists on producing a value that's very
difficult to generate but fairly simple to verify, furthermore, it must meet
certain criteria in order to be considered valid. This value is used for block
generation, which comes with a reward in the form of coin, that's why nodes on
the network compete to be the first to find it. This process is what's referred
to as mining.

On the other hand, proof of stake based algorithms try to reach consensus on
block generation based on a validator's wealth (stake). There are several
implementations, for example, Nxt and Peercoin have their own chain-based proof
of stake approaches based on randomization to select which validator gets to
create the following block. In contrast, Ethereum will use a Byzantine
fault-tolerant based proof of stake system, in which validators get to propose and
cast votes to select the next valid block (The first iteration of Casper
will only allow selection of already existing blocks, see the next section for
more details).

## Casper FFG's PoW/PoS Hybrid approach

Initially, Casper will use a mixture of both methods, block generation will
continue to be through PoW (Mining), and PoS will be used for selecting
which blocks will become what has been defined as **checkpoints**.

### Validators

The new system comes with an additional set of players, the **validators**,
they are the miners counterpart in PoS. To become one we have to
make a deposit of some minimum amount into the system, in the testnet is
currently 1500 eth, this deposit is what's referred to as the validator's stake.
As long as we are active validators we cannot use this deposit for
other purposes, to withdraw it we have to issue a logout command; keep in mind
that this is not an immediate action, it will take some time, at least two epochs
(1 epoch = The period of time between two checkpoint selections).

Validators participation on the network is rewarded as an increase in their deposits;
just the same, if they behave badly, breaking any of the slashing conditions we
will see later, the system will penalize them with a decrease of their deposit
money.

Just like it currently is, we will have our energy-hungry miners competing for
creating new blocks, this is nothing new, but, after 100 new blocks are
added to the chain, the validators will start submitting votes to select a
new checkpoint. For a block to become a checkpoint it must receive 2/3 of the
votes as defined in the whitepaper:

> Proof of stake’s security derives from the size of the deposits, not the number
> of validators, so for the rest of this paper, when we say “2/3 of validators”,
> we are referring to the deposit-weighted fraction; that is, a set of validators
> whose sum deposit size equals to 2/3 of the total deposit size of the entire set
> of validators.

### Checkpoints

As stated before, every 100 blocks a new checkpoint will be selected by the
validators. A checkpoint passes through two stages, first it is justified, which
basically means that it has received at least 2/3 of the validator's votes and it
belongs to the checkpoint tree; the other is when it's justified and one of
its direct children (the 100th) gets justified, granting the checkpoint the
status of finalized.

Justified and finalized checkpoints play major roles in the system because
Casper introduces a new fork choice rule that replaces the standard
PoW rule of "always build atop the longest chain" with "follow the chain
containing the justified checkpoint of the greatest height", with the height
defined as:

> the height h(c) of a checkpoint c is the number of elements in the checkpoint
> chain stretching from c all the way back to root along the parent links.

Also, an extension is added for preventing a type of attack that takes advantage
of withdrawal delay and historical data (long range revisions), this
extension comes in the form of a rule to never revert a finalized block.

### Slashing conditions

Casper validators must adhere to two rules or slashing conditions pertaining
the voting process:

1. A validator must not publish two distinct votes for the same target height.

2. A validator must not vote within the span of its other votes.

These rules provide accountable safety and plausible liveness, topics we won't be
discussing in this post, but if you want to get more information be sure to check the
[whitepaper](https://arxiv.org/pdf/1710.09437.pdf) and these two posts
[Minimal Slashing Conditions](https://medium.com/@VitalikButerin/minimal-slashing-conditions-20f0b500fc6c),
[Formal methods on some PoS stuff](https://medium.com/@pirapira/formal-methods-on-some-pos-stuff-e309775c2ab8).
The key point here is that breaking any of these rules will result in a penalty
on the validator deposit.

Inactivity leak is another way of being of being penalized:

> slowly drains the deposit of any validator that does not vote for checkpoints,
> until eventually its deposit sizes decrease low enough that the validators who
> are voting are a supermajority

This is a defense mechanism in case more than 1/3 of the validators do not cast
votes (could be due to a netsplit, error or an attack), making it impossible
to reach consensus on which will be the next checkpoint and getting the system
stuck.

## Final thoughts

This is just the first phase of Casper, it will serve as the foundation for a
fully proof of stake system, which is something the Ethereum's community has
been working on for a long time.

One of the main advantages with proof of stake is that it will decrease
dramatically the environmental footprint by moving away from the high energy
consumed by proof of work mining; you can look at it like getting an electric car
to replace an old diesel truck. Casper will also solve the nothing at stake
problem associated with proof of work, discouraging attempts on attacking the
system by adding economic penalties.

There is still a long way to go, remember, this is just the first of many
iterations, meaning that there is a lot of room for improvement. It should be
interesting to see how the system behaves and evolves once it goes live.
