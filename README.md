Preamble
------------
The specification for Moneysocket is work-in-progress.

The existing library implementations have been an exercise in figuring out the problem space and gathering experience and opinions on what we're building.

The exercise of writing the first draft of the spec is more an exercise in revisiting all the choices made along the way of building the libraries and existing apps. Some choices have been great, others less so.

The exact shape and size of this deliverable is TBD, however inspiration is taken by some of the other documents and processes in the Bitcoin space for BIPs, BOLTs, LNURL-XXXs, etc.

This is also an effort to bootstrap and sustain an open and transparent process for all stakeholders for the long term. If you have any thoughts, opinions, questions, please drop in our [Telegram Group](https://t.me/moneysocket) or file issues or whatever you feel comfortable with.


Software Model
------------

Some conceptual foundation of Moneysocket when it comes to intent and language-independent software architecture.


* [Consumers and Providers](software-model/consumers-providers.md)
* [Accounts](software-model/accounts.md)
* [Composibility](software-model/composibility.md)
* [Stack/layer/nexus model](software-model/stack-layer-nexus.md)
* beacon/relay behavior
* transport layers (outgoing websocket, incoming websocket)
* Code API description


Basis Of Moneysocket (BOM)
-----

This borrows structure from [Basis of Lightning Technology](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) (BOLT) specification.

These articles will capture individual aspects of the techonology in implementable detail.

* [BOM #0](00-introduction.md) - Introduction
* [BOM #1](01-encoding.md) - Encoding format primitives (kinda like bolt1 & bolt 4)
* [BOM #2](02-beacons.md) - Beacon encoding format (kinda like bolt11)
* [BOM #3](03-provider-info.md) - Provider Info data & Wad data
* [BOM #4](04-messages.md) - Request/notification message encodings
* [BOM #5](05-transport.md) - Transport layer and nexus behavior
* [BOM #6](06-rendezvous.md) - Rendezvous layer and nexus behavior
* [BOM #7](07-ephemeral.md) - Ephemeral Encryption Layer
* [BOM #8](08-identity.md) - Cryptographic Identity Layer
* [BOM #9](09-roles.md) - Consumer/Provider/Automatic role handshake layer
* [BOM #10](10-api.md) - API layer
* [BOM #11](11-errors.md) - Error Messages

Moneysocket Improvement Proposals (MIPs)
-----

This is a space for BIP-like feature and extension proposals to be proposed, discussed, refined and implemented in standardized ways without necessarily getting includsion in the Basis of Moneysocket documentation.

* [MIP #0001](mips/mip-0001-avatar-image.md) - Avatar Image for Public Key
* TBD
* TBD

Appendices
------------

* [Shortcomings](appendix/shortcomings.md) - Known shortcomings of the pre-specification library implementations.
* [Embedded Relay](appendix/embedded-relay.md) - Suggestion on how to implement a listening Moneysocket servers


Project Links
-------------

- [Homepage](https://socket.money)
- [Awesome](https://github.com/moneysocket/awesome-moneysocket)
- [Twitter](https://twitter.com/moneysocket)
- [Telegram](https://t.me/moneysocket)
- [Donate](https://socket.money/#donate)
