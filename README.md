The Spec
------------
The specification for Moneysocket is work-in-progress.

The existing library implementations have been an exercise in figuring out the problem space and gathering experience and opinions on what we're building.

The exercise of writing the first draft of the spec is more an exercise in revisiting all the choices made along the way of building the libraries and existing apps. Some choices have been great, others less so.

The exact shape and size of this deliverable is TBD, however inspiration is taken by some of the other documents and processes in the Bitcoin space for BIPs, BOLTs, LNURL-XXXs, etc.

This is also an effort to bootstrap and sustain an open and transparent process for all stakeholders for the long term. If you have any thoughts, opinions, questions, please drop in our [Telegram Group](https://t.me/moneysocket) or file issues or whatever you feel comfortable with.

Brain Dumps
------------

* [Shortcomings](shortcomings.md) - known shortcomings of the pre-specification library implementations.


Software Model
------------

* beacon/relay behavior
* stack/layer/nexus model
* transport layers (outgoing websocket, incoming websocket)
* Code API description


Basis Of Moneysocket (BOM)
-----

This borrows structure from [Basis of Lightning Technology](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) (BOLT) specification.

These articles will capture individual aspects of the techonology in implementable detail.

* [BOM #0](00-introduction.md) - introduction
* [BOM #1](01-encoding.md) - encoding format primitives (kinda like bolt1 & bolt 4)
* [BOM #2](02-beacons.md) - beacon encoding format (kinda like bolt11)
* [BOM #3](03-provider-info.md) - Provider Info data & Wad data
* [BOM #4](04-messages.md) -request/notification message encodings
* Websocket Layer
* Rendezvous Layer
* Ephemeral Layer
* Auth Layer
* Consumer Layer
* Provider Layer
* Consumer API Layer
* Provider API Layer
* Relay Layer

Moneysocket Improvement Proposals (MIPs)
-----

This is a space for BIP-like feature and extension proposals to be proposed, discussed, refined and implemented in standardized ways without necessarily getting includsion in the Basis of Moneysocket documentation.

* [MIP #0001](mip-0001.md) - Avatar Image for Public Key
* TBD
* TBD


Project Links
-------------

- [Homepage](https://socket.money)
- [Awesome](https://github.com/moneysocket/awesome-moneysocket)
- [Twitter](https://twitter.com/moneysocket)
- [Telegram](https://t.me/moneysocket)
- [Donate](https://socket.money/#donate)
