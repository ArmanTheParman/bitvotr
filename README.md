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

<p align="center">
<img src="https://github.com/ArmanTheParman/bitvotr_protocol/blob/c91ded00458ac95111f7e65151e9d02087cd2a40/RAFT_levles.png" alt="RAFT cluster levels schema" width="50%;"/>
</p>
<p align="center">
<em>Leaders, shaded, in clusters of level 1 become nodes in the level above and so on. Not all nodes shown, and not all levels shown.</em>
</p>

The system can handle people dropping out and reconnecting, and can handle low computing power. If the total pool of voters was 300 million, then there would be 17849.7 megaclusters. The incomplete cluster (0.7) can be managed by merging it deterministically and evenly with the others.

As long as a cluster has 4 nodes out of 7 alive, the '[majority rule](#majority)', holds, and the cluster can remain active and connected to lower and higher RAFT levels. When nodes that disconnect and don't reappear in time, the remaining nodes flag their ID's, or the ID of their entire section if a section goes down, to a shared "dropped list" via a gossip protocol.

Time Period 2

At the beginning of this period, nodes will be linked to one of the megaclusters. It is possible that some nodes will not have participated at all, or are disconnected at this point in time for whatever reason. They will have until the end of time period 2 to join the megacluster. If they don't reconnect in time, they still have the opportunity to vote in the 'dropped round' or final round.

During this time period, votes are exchanged and verified within the RAFT cluster of 7 (level 1 of the megacluster), then the leader nodes of the clusters merge the votes into a NOSTR data structre which is then signed as a whole by each member in the cluster.

The merged signature is considered "approved" if it passes the Byzantine Fault Tolerance standard, which is more strict than the 'majority rule' which is applied to the connectioin status of the RAFT cluster.

The leader of the cluster then programatically signals to each member that they can delete the individual votes (for enhanced privacy). Once done, the cluster signals their work for Period 2 is complete.

Clusters that fails to reach completion status by the end of Period 2 (ie failed the Byzantine Fault Tolerance standard) are added to the 'dropped list' - that is, the entire cluster of 7 nodes no longer participates in the pyramid for the next time period.

Time Period 3

The leaders of each level 1 cluster (base of the megaclster pyramid) are members of the level 2 cluster "above" as well. One of the seven in level 2 becomes the leader and 6 others become followers (they still remain leaders of the level below of course, otherwise they drop down and get replaced). Note the leader of level 2 is also potentially a leader of levels above as well.

The level 2 leader co-ordinates the merging of each of the 7's (self included) merged Nostr-Tally meged vote, and at the end of the round, they should all be holding a merged-vote of 49 votes, and at least 33 signatures from the lower levels. Thirty-three is the minimum required for Byzantine Fault Tolerance. If it is achieved, the group can progress to time Period 4. Otherwise, the entire group's 49 voters are added to the dropped list.

Time Period 4

During this period, the group sizes are 343 in total (7^3, or 7x7x7 = 343) if none have been dropped. 

In level 3, a leader is in a group with 7 other members. Each of them are leaders in their respective level 2. And each of those are leaders in level 1.

The level 3 members coordinate, together with their leader, a data structure with 343 votes in total. If 229 votes are collected by the end of Period 4, then the group has an approved merged Nostr-Tally and proceeds to the next Time period.

Time Period 5

The Nostr-Tally has up to 2401 members and needs 1601 signatures to be approved.

Time Period 6

The Nostr-Tally has up to 16807 members and needs 11205 signatures to be approved.

Stage 3 - Voting Round 2

The dropped list - Time period 1 to 6

The list can be extracted from any node as they are all in sync. They are all arranged in an apparently random but deterministic way like the first list. The pyramid structrue will be the same as before, and the same stages will happen as before. Any that drop again or don't show up are added to the final list - shared with all nodes.

One important difference in this round is that if a pyramid reaches the size of 2401, then failure to merge into the final 16807 does not drop the entire group into the final list. Instead, the 7 approved clusters of 2401 remain as is, and attempt a merge in Voting round 3. This design is so that the opportunity to vote doesn't rapidly diminsh due to connection problems, while still maintaining a reasonable amount of anonymity.

Stage 3 - Voting Round 3

The final list - Time period 1 to 6

Like the dropped list in voting round 2, the tolerance for failure during this round is also reduced. This time, ANY successful merge is accepted, and clusters that fail to merge simply stop trying and reach their final form.

Stage 3 - Voting Round 4

No merging is done in this phase. Any node that hasn't cast a vote should do so, and stay online. Their vote will have any privacy from the voting coordinator. Users can decline this and send in a standard vote outside of the BitVotr system.

Stage 4 - Data Publication

Every node shall hash the tally it is holding and share the hash in a gossip protocol. All nodes generate a list of hashes representing the vote tallies, and add unique hashes to their list,  'voting hash list'.

From this point on, the data is disseminated in 3 ways:

Every node that wishes to verify has the voting hash list and can use it to connect to nodes and request data exchange.

The voting tally, WITH its hash in an appropriate field can be published to NOSTR and shared between NOSTR relays. Anyone can search for the hash and receive the tally. The signatures are also published as separate NOSTR events - they are the signatures of the tally data. The event has the hash of the tally, the pubkey they are signing for, and the signature data. Extra NOSTR relays during the election would assist dissemination.

Nodes can also share the merged tally and signatures over BitTorrent. BitVotr will prepare the files for sharing, prepare the torrent file, share to a tracker. Those who are verifying the election can then search for the hash and the tracker will direct them to the download.

By getting all the tallies the vote can be quickly counted, confirmation requires the downloading and verifying of all the signatures as well. It's up to the verifier to decide how many signatures they want to collect to confirm the tally,  in order to be satisfied the election was honest.

Appendix

Why the name, BitVotr?

The name is a mixture of Bitcoin, Voting, and Nostr, reflecting the design. While no data is required to be published to the Bitcoin timechain, it is used as the election clock, and also to extract future sufficiently-random data (block hash) that all voters can unanimously agree on. Nostr is used in the publication phase of the election to widely disseminate signed votes for anyway to download and verify.

How BitVotr borrows from Bitcoin and Tor private/public keys

In Bitcoin, a BIP39 seed phrase is a protcolised way to encode a large random number into words, typically 12 or 24. The words are converted to ASCII bytes (these are integers), and then it goes through a series of steps including a hash to generate a 512 bit private key. 

At this point in the chain of events, public/private key cryptography is possible. Before this, the system is just a protcol to get to the private key, and done for ease of human recording with minimal chance of error.

Private keys are kept private and they produce public keys.

Private keys sign digital data/messages, and produce digital signatures (large numbers). People with private keys can demonstrate to others, the verifiers, that "the private key of the public key I gave you, has produced this signature" - and the others can verify easily that it's true.

In Bitcoin it gets really interesting partly because the public key is inside the message being signed.

In BitVotr, the system of signing with private keys is used but the keys are slightly different. They are borrowed from the Tor network, not Bitcoin. Tor uses onion addresses, which are in fact public keys. While the keys are used for verification and encryption, they are also communication endpoints; ie secret IP addresses. When used in BitVotr, direct P2P communication with the public key owner is possible, and exchanging signatures directlybecause possible.

To generate onion addresses, private keys are normally generated by the Tor program. BitVotr will bypass this and create it's own onion private keys, using a custom protocol incorporating BIP39. BIP39, with words, creates a 64 byte integer as the seed, before making keys. BitVotr will do the same but in the last step will generate a 32 byte integer (using HMAC-256 instead of HMAC-512), and from that integer, will generate the private key. The cryptography for v3 onion address
