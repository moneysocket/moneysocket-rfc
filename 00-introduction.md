# BOM #0: Introduction and Index

These Basis of Moneysocket (BOM) documents describe a protocol for connecting applications for the purposes of coordinating the transfer of satoshis for services and/or goods delivered by one or more application in return.

1. [BOM #1](01-encoding.md) - encoding format primitives (kinda like bolt1 & bolt 4)
2. [BOM #2](02-beacons.md) - beacon encoding format (kinda like bolt11)
3. [BOM #3](03-provider-info.md) - Provider Info data & Wad data
4. [BOM #4](04-messages.md) - request/notification message encodings
5. more...


As you may have noticed, the structure of these documents is heavily influence by the [BOLT](https://github.com/lightningnetwork/lightning-rfc) specification for the Lightning Network.


## A Short Introduction to Moneysocket

Moneysocket is intended to be spelled as `Moneysocket` and not `MoneySocket` in a bold attempt to be cool like the movie [Moneyball](https://en.wikipedia.org/wiki/Moneyball_(film)) and to assert itself as a proper noun instead of a goofy neologism. The term applies to both the protocol and an individual socket connection instances that uses the protocol.

A Moneysocket, as the name implies is a [network socket](https://en.wikipedia.org/wiki/Network_socket), but intended to transfer money as it's ultimate content as opposed to communication like a traditional socket.

The Moneysocket protocol, as defined by these series of documents, performs a handshake between the participants on each end to establish the connection and context. From there, the Consumer end of the connection is able to make requests to the Provider end of the connection to generate and pay Lightning Network invoices among other things.

The Moneysocket protocol is independent of programming language and is suited for every language where incorporating Lightning Network payments might make sense.

### Protocol Versioning

The protocol reserves the `Major` version number of software to specify the revision of this specification.

This protocol version number is expected to be revised extremely infrequently (perhaps never). A pre-existing example of how this is intended to be used is the difference between Python2 and Python3, where there's a need for major breaking changes, and afford the code the option of staying with the previous iteration.

### Version 0 - Pre-specification

At the time of this writing, reference implementations have been created to explore this problem and inform this specification. It has used `0` as its major protocol version. This was never intended to be used in production use cases and version 0 will be retired after this specification and Version 1 code is released.

#### Version 1 - First post-specification release

This is the version of this specification expected to be accompanied by the first stable release of reference implementation libraries.

#### Version 2+ - Reserved for major revisions in the future

If there is a need identified for a fundamental structural change to this specification, the versioning will proceed by incrementing this number.


## Software Release Versioning

When protocol implementations libraries are created and released, they must specify their version according to `Major`/`Minor`/`Patch` convention with the protocol version in the place of the `Major`.

It is intended that reasonable lowest-common-denominator compatibility can be expected between two implementations of the Major protocol version.


## Protocol Extensibility

The specification is intended to allow for implementers to add their own features and extensions without any prior approval. These implementations is expected to precede the inclusion into this specification. The implementation of additions can also freely exist without inclusion. It follows that additions to the specification can be made, but an implementation can opt to not implement. A design goal in the details is to accommodate this process as well as can be done.

It is the goal to eventually include all useful, well-considered, and well-designed things into the specification, provided reasonable consensus can be obtained through and open and fair process.


## Glossary and Terminology Guide

#### Application

#### Consumer

#### Provider

#### Bidirectional

#### Account

#### Balance

#### Stack

#### Layer

#### Nexus

#### Beacon

#### Shared Seed

#### Rendezvous ID

#### Location

#### Wad

#### Provider Info

#### Message

#### Notification

#### Request

## Theme Song

The BOLT spec has a theme song. We don't... yet.



## Authors

[ FIXME: Insert Author List ]

![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png "License CC-BY")
<br>
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
