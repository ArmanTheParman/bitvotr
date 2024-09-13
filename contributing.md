# Contributing

Thank you for contributing to this project. 
This page will be a frequently updated, and serves as a starting point for things that I need to do, and things that I could use help with.

### Review Whitepaper
Suggestions to imporve readability are welcome via pull requests.
I will split the Appendix into a Q&A and proper appendix.
The good questions I'm responding to in private comments will be added (including any asked in the GitHub issues sections).

### DoS considerations
Currently, I have voters initiating a request to vote by creating a public/private key and submitting a photo of themselves holding the public key. 
This allows the voting coordinator to prove a citizen has submitted a vote, and defends against fraudulent accusations.
However, there is a risk that the coordinator can create fake evidence and fake keys, denying real voters the chance to vote - complaints can be
met with fake images by the coordinator. In order to solve this, I am going to add a new feature - Proof of Tax.

### Proof of Tax
The introduction of a public key used in association with an existing government identification, eg a social security number or tax file number.
If the payment of tax is linked to the public key, for example, if tax returns are signed with the private key, then whichever public key
is being used by the citizen is the one the government has an incentive to acknowledge as being the real public key. Using keys in this way
also eliminates the need for leaking privacy - eg no need to publish your SSN or your public key to the general public, or a web-of-trust, the
latter being a privacy problem because it leaks information about who one associates with.

If trying to fraud an election making up public keys and AI images will only allow them to create voting public keys that have no link the
the cititzen's tax history. If there is a dispute, then fake images will not hold up in court. 

With this modification, BitVotr photo identity can be done away with, and instead, voters can register their public key in a citizen identity 
scheme not directly part of BitVotr, and done in advance. The system would exist for some time before BitVotr can use it.

### Networking
Currently, the protocol specifies using Tor for peer-to-peer communication. This will only work for small elections. If scaling to 300M
people, as I understand it, the Tor network will not be able to tolerate the required bandwith, and nor will it tolerate the creation
of 300M hidden services.

When considering an alternative, it's important to recognise the need to hide IP addresses, as they are associated with identidies. When a
public key and and IP come together, too much information is revealed about a voter, and their vote can be exposed, or at least, there will
be the concern by voters that it could be exposed.

As a potential candidate, I am considering HolePunch, a system used by Keet. I need advice about how and if IPs will be hidden.

Another is Veiled, which as I understand won't have the IP address problem, but I am unsure if it can scale.

### RAFT
The Raft protcol is well described and a desicion needs to be made about wheter to create libraries from scratch or use existing libraries
and from which programming language. This might influence which language to use for the BitVotr app overall.

