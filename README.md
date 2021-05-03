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


Basis Of Moneysocket (BOMS)
-----

This borrows structure from [Basis of Lightning Technology](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) (BOLT) specification.

These articles will capture individual aspects of the techonology in implementable detail.

* [BOMS #0](00-introduction.md) - introduction
* [BOMS #1](02-encoding.md) - encoding format primitives (kinda like bolt1 & bolt 4)
* [BOMS #2](02-beacons.md) - beacon encoding format (kinda like bolt11)
* [BOMS #3](03-wad.md) - Provider Info data & Wad data
* [BOMS #4](04-messages.md) -request/notification message encodings
* Websocket Layer
* Rendezvous Layer
* Ephemeral Layer
* Auth Layer
* Consumer Layer
* Provider Layer
* Consumer API Layer
* Provider API Layer
* Relay Layer



Project Links
-------------

- [Homepage](https://socket.money)
- [Awesome](https://github.com/moneysocket/awesome-moneysocket)
- [Twitter](https://twitter.com/moneysocket)
- [Telegram](https://t.me/moneysocket)
- [Donate](https://socket.money/#donate)
