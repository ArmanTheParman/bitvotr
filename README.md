# BitVotr

## What is BitVotr?

***PROBLEM***: How can votes in an election be counted in a provably honest way (provable by anyone) without revealing the votes of individuals publicly, or from the government?

***SOLUTION***: BitVotr is a P2P computer protocol that can be used by democracies to run elections that result in a provably honest count. Voters can vote from the privacy and comfort of their homes with their computer, and can verify the total count is correct and their own vote has been counted.

## Aims

Achieving the following three aims simultaneously is the innovation of BitVotr:

- Election provably not manipulated (anyone can verify the total count is honest)
- Each voter can verify their vote was counted
- Voter privacy from citizens, election coordinator, and government.

An unintended benefit of BitVotr is the massive cost savings of running an election.

## Overview

Each voter has a unique identifier which is a public key (for vote publishing), and also doubles as an onion address for P2P communication, and voting network organisation.

Voters use an open-source BitVotr app to connect online to the network, and verify the growing tally of votes – they can verify their own vote is included in the tally, that every other vote is valid, and that there are no double votes.

To accommodate those without technology access, a parallel regular vote – in person or mail-in, should be made available.

## Eligibility

In current democratic systems, all voters need to prove some sort of eligibility to vote.

Although I have designed a way to bypass the need for an election coordinator, BitVotr’s initial aim is not to change the rules that any democracy has set up – eg voting age, citizenship, maximum single vote, equal power of votes etc. Eliminating the need for a central authority to organise voter eligibility is not the purpose of BitVotr, and is not something that has been solved to date. In any case, this is not a critical problem with the election process – the problem is how to run an election openly so any suspicious activity is publicly apparent, and can illegitimise the claim of victory.

Actually eliminating the need for a coordinator that oversees who is eligible to vote (eg there’d be a need to exclude foreigners and perhaps have age limits), would open the door to very different kinds of elections – perhaps those that can be called by anyone, not just by the system of democracy in the country; a true peoples’ election. There is a way to achieve this, but it would need to be a modification to BitVotr, not an alternative. It is discussed in the [appendix](#edid).

BitVotr uses the democracy’s status quo methods to ensure no one is given more than one “ticket to vote”, but introduces public/private key cryptography to prevent anyone impersonating a voter, changing anyone’s vote, destroying votes, or tampering with the tally.

In order for a voter to be approved/identified, they are to submit (as a potential example) the following to a voting coordinator:

A photo ID of themselves holding their ID AND their self-generated public key (The private key is kept secret. The BitVotr app will generate the key for them). Producing this document is how it can be ensured that one identity gets only one vote.
Optionally, depending on the perceived risk of photo fakery, a video submission could be added, the video hashed, and the hash added to the photo. The election coordinator then uses these submissions to make an approved list of public keys permitted to vote, one per recorded ID.
Those who decline the BitVotr system (or are unable to use it) can vote the manual way as before (in person, or mail – it’s up to the democracies to decide that for themselves), but will lose their ability to verify their vote was counted. Regardless, voters will still need to undergo the previously used eligibility process to prove they are eligible to vote.

## Equipment necessary
Windows, Mac, or Linux computer, or access to a trusted person’s computer. Combining resources (multiple accounts on one computer) is allowable/possible, up to 10 per modern day computer (this could be changed after results from testing).

Minimum computing power equivalent – a Raspberry Pi 4

Minimum drive storage required 50 GB (initial estimate) – external USD hard drive possible. Smaller storage is acceptable if the voter does not wish to verify the total count themselves.

Broadband internet connection with access to the Tor network

## Method:
The Voting event is made up of three stages.

- Stage 1 – Preparation
- Stage 2 – Entering the Vote
- Stage 3 – Four voting rounds (submitting votes)
- Stage 4 – Publication
## Stage 1 – Preparation

An open-source app, the BitVotr app, is made available for download. Using the app, voters will generate a random 12-word seed which is used to produce their private key and public key (see basics of [seeds and keys](#ppkc) in the appendix).

The private key is stored in the app and is password protected. The 12-word seed should be physically written down and kept safe, to allow restoration of the private key if needed. If the private key or the app is lost, it can be regenerated with the 12-word seed, but losing both is unrecoverable – while that is regrettable, it is not catastrophic; it means losing the ability to vote with BitVotr, but a physical vote may still be available, depending on the specific election arrangement.

Stolen keys can be cancelled on request to the coordinator with whatever manual ID verification they require. Cancelling a stolen key means the thief can’t vote, but also, the ability for the “victim” to vote must be cancelled – as being permitted to vote would otherwise introduce the risk of double-voting fraud.

**The Public Key List** of every registered voter is to be published well before the election date, allowing anyone to check their application to vote has been accepted. Once on the list, the ability to vote is assured.

The list allows anyone to check the maximum number of votes that can be cast (equivalent to the number of keys on the public key list), and compare that with the size of the voting population they believe is true.

It is up to the election coordinator that no one has a double-vote, as is done currently, but if defrauded to any significant extent, the number of public keys would then exceed the anticipated number of eligible voters.

No one but the coordinator at this stage can know which key belongs to who, but this potential privacy weakness will be dealt with during the voting process, as will be shown during the explanation of the voting rounds. (Also see: [Why merge votes?](#whymerge) and [Why RAFT clusters?](#whyraft)).

If there are disputes raised that someone’s key is not published, then this can be settled with the photo ID and signature that was initially submitted. Although identities can be faked by using AI-generated images (resulting in excessive keys to cast votes), if done to any significant extent, the fraud becomes undeniably apparent before the election has started.

*This is consistent with the aims of BitVotr, which, stated in another way, is to make it difficult to hide the true nature of fraudulent elections.*

During stage 1, the conditions of the election are published by the coordinator.

The start date shall be set to the Bitcoin network’s clock, ie a Block Height, for very important reasons that will become apparent later. Provided humanity is not on the brink of extinction, the Bitcoin clock is almost as reliable as the sun coming up.

Variables for the election, such as the duration of voting rounds will be written into the BitVotr App code, and made available for download. The software will also be signed allowing voters to verify that they have a true copy of the correct version. Instructions to do this should be clear and easy.

## Stage 2 – Entering in the vote

Prior to the election, all voters should access their BitVotr app and run the pre-election program:

1) Select their vote, and sign the vote with the digital private key (all made easy by pressing a button in the app).

2) Acknowledge the Bitcoin block to start the election, to be ready and online at the time

3) Download the Pubkey List (large file)

4) A completion signal could be sent to the co-ordinator (might not be necessary)

5) BitVotr will create a Tor hidden service using the private key, which results in the onion address server endpoint.

## Stage 3 – Voting Round 1

### Time Period 1

Time periods are counted in Bitcoin blocks for easy agreement of time, and no need for central coordination.

The election begins at the pre-specified Bitcoin Block height, which marks the beginning of time period 1. This period lasts an arbitrary number of blocks based on network factors, yet to be determined.

The BitVotr app extracts the Bitcoin block and obtains its hash. The hash is then used to deterministically mix the order of the PubKey list ([using prime finite fields](#mixingkeys)).

Peer to Peer connections over Tor are made in [RAFT-protocol clusters](#raftclusters) of seven (See [why RAFT clusters](#whyraft)?). The voters’ public keys are also their [onion address for the Tor network](#onionkeys).

[Megaclusters](#megaclusters) of 16,807 nodes are formed (takes seconds), and maintain communication via levels of hierarchical RAFT protocol clusters explained later.

<img src="[path/to/image.png](https://github.com/ArmanTheParman/bitvotr_protocol/blob/c91ded00458ac95111f7e65151e9d02087cd2a40/RAFT_levles.png)" alt="RAFT cluster levels schema" width="300"/>

<p align="center">
  <em>Leaders, shaded, in clusters of level 1 become nodes in the level above and so on. Not all nodes shown, and not all levels shown.</em>
</p>
