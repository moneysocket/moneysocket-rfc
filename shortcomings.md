This is a capture of a bunch of stuff that is known to be deficient in the pre-specification implementations.

# Big ommisions

A couple things that are missing from the implementation that are very desirable features.

Feature-creep is also a problem, but these things are conceptually important to the message of Moneysocket.

#### B1 - Public-key crypto layer

* Replay attacks from the relay are probably possible
* man-in-the-middle attacks are easier than they need to be
* Cryptographic identity is missing (useful for Nostr, Moneysocket Chat, lnurl-auth style applications etc.)
* Encryption of messages (Nostr DMs, other chat systems).

* Diffie-Hellman exchange
* ProviderInfo announces key set and types of signatures that can be performed.
* Layer of block cypher encryption
* Sign message call
* Encrypt call

There needs to be an ephemeral encryption layer to prevent logged cyphertext from being decrypted later if the key/SharedSeed leaks. Also, an identity layer that has persistent public keys that can do auth/signing/encryption tasks.



#### B2 - WebRTC

There is a criticism that the websocket-based moneysocket connections require the relay to work best. This adds latency and a tendency to depend on centralized hosted infrastructure.

When performing a 'rendezvous', sometimes we can have endpoints perma-listen, waiting for the other side to join, but other times we need to manually send in each side to meet.

With this relay scheme, it feels like we are just re-inventing WebRTC badly. The desired abstraction is like a peer-to-peer 'phone call' to take a direct route from point-to-point. This is an hint/indication that we should use literal telephony technology to establish the socket.

WebRTC is still relatively new and underexplored, but it seems to be a fit for the mission.


# Medium-sized issues

#### M1 - Beacon rendezvous setup is error-prone

There's a directionality/gender to the beacon connection that's not obvious. Common to some standards/protocols (USB 2.0, ethernet crossover, LNURL (SERVICE vs WALLET), etc.)

* At the very least, need to produce a descriptive error message when connecting th wrong way.
* Perhaps hint in the beacon what 'gender' it's expecting to join
* Perhaps put a symbol for the 'gender' in the QR code

#### M1 - KeySend is desired

For the [value4value podcast index tags](https://github.com/Podcastindex-org/podcast-namespace/blob/main/value/value.md), they specify a node endpoint for you to KeySend value. This scheme has controversy since bolt11s provide proof-of-payment and the KeySend scheme does not (this is confusing for both sender and receiver about 'why' the payment is happening). However, it does have some attraction and providing KeySend as a moneysocket request isn't a monumental amount of effort.

This would at least allow us to implement a demo `value4value` application that conforms to this standard.

#### M2 - Announcing nexus to application does not come with ProviderInfo

ProviderInfo comes in a different call later, so a nexus isn't fully online and useful to the app upon being announced. Fixing this would make the library API simpler.

Perhaps a "Managed Provider" layer to hold back the nexus until the provider info is fully ready.


#### M3 - Multiple paths to the same provider are not de-duped

When the Provider Info data structure is provided from multiple connection paths, two different nexuses are presented to the app.

One intention with the nexus design was to accommodate multipathing and high availability. Redundant links to the same provider/account can be round-robined and swapped out non-disruptively.

The solution is to have a consumer layer detect the redundant provider info and bundle them into a single nexus provided to the layer above.

#### M3 - Accounting for routing fees not done well

`NOTIFY_PREIMAGE` is sent back both after an outgoing and incoming payment.

For outgoing, it comes back with what the routing fee was, but that's not propagated upstream for the user to use.

Also, the user might want to have some control over what is an acceptable routing fee to pay for an outgoing payment.

There needs to be at least two different styles of notifying a preimage - one for outgoing and one for incoming payments. The outgoing version should carry the paid routing fees in performing the payments, such that balances can update accordingly and exactly.

#### M4 - Not entirely sure the best way to do protocol version numbering

* LNURL doesn't have a protcol versioning scheme
* Bitcoin itself doesn't have protocol versioning scheme
* LN BOLTs doesn't have protocol versioning scheme

However, there is some upgrading and extensions that need to happen via some process. It's naturally a bit anarchic, so the tendency is to want to control it and be clear about compatibilities, however that's really quite difficult when unppacked.

It seems that the protocol needs to 'free float' to an extent. Libraries denominated by the software release version, but it needs to be cross-compatible with different library versions. Some extensions to the protocol can be slipped in without pursuing too much 'blessing', but the core of the project needs to be stable and extensible to accommodate that.

I think a vision is to use the Major version number (eg. the "1" in "v1.2.3") to represent extremely infrequent (perhaps never) breaking protocol changes. An example would be Python3 vs Python2. Within a version, we need to sure there is mostly internal compatibility for the core services so a library relase v1.2.3 can work with v1.3.3 without too much trouble, however features that might be available in the later version won't be able to be used.

This probably means the first post-specification release of the moneysocket libraries will be v1.0.0. If there are significant flaws discovered with v1.*.* down the road that need some re-architecting in a breaking way, that'll be v2.0.0.


#### M5 - Message format and versioning scheme could be better tuned for extensibility

The version numbering should be more explicit in the message, and probably best to do that in binary as part of the initial header rather than buried in the JSON. The "it's OK to be weird" strategy of the TLV formats in the BOLT specs are also informative. This can allowing and encouraging extension and experimentation without hurting the core protocol.

Binary-header-with JSON-interpreted blobs appears to be a good way. Get the version and the 'little' stuff in the binary. Anything more complex can be put in JSON where it's easy to add fields later and such. Also, allowing some binary extensions can't hurt.

|-version-|-T-|-L-|-JSON info-|-T-|-L-|-Extension Data-|-more TLVs-|

If there is a need for a major overhaul later, incrementing the version number can inform the parser.

#### M6 - re-using SharedSeed values leads to undefined behavior

It's possible to choose re-used SharedSeed values in a way that could confuse apps. Not sure if there is a valid use case, but in the meantime, enforcing uniqueness should be coded into the spec and all the apps should have a directory of in-use values that allows it to reject a new entrant that has an inappropriately re-used value.

#### M7 - re-using Preimage values leads to undefined behavior

It's possble for a participant to define preimages that are not purely random values. This might be valid behavior where re-using values or using values with mathematical relationships might be part of a scheme for chaining invoices to trigger based on other invoices triggering.

This needs to be formally described and codified in the spec so the apps have some guidence on how to handle these in the right way.

#### M8 - Max withdraw is an issue

By design of how the 'pay' works on LN, the routing fee is not known until the payment is fully complete. This makes it a bit awkward to reduce an account to hard zero.

We likely need some 'reserve' amount for this problem where the account logic will decide an account is zero and set it as such when the balance goes below.

A descriptive error for invoices that are just barely too big for the account to successfully pay, after incorporating the fee, would be helpful to allow the user to get it right on the second try.

Also, perhaps a special command/process to 'withdraw full balance' with an ammountless invoice should be implemented for this particular case. That might require some work into customizing the 'pay' process (this is a plugin in c-lighting) to pay under more custom parameters to be most efficient.

#### M9 - Error messages need more structure.

Right now, the error messages on `NOTIFY_ERROR` is just a free-form string. However, some standarization for common failures would help immensely to provide guidence for implemenations.

However, allowing free-form strings for things that don't fall under the 'official' set of errors is still necessary.

# Small-ish issues

#### S1 - Provider/Consumer Transact Layer is poorly named

There's Provider/Consumer layer and then Provider/Consumer 'Transact' layer that are confusing to have side-by-side. Suggest "API" layer as a naming basis. "API Provider Layer" & "API Consumer Layer".

The API provider layer consumes services from the app to provide the API to the remote API consumer layer. The API consumer layer calls into the API to orchestrate payments.

This will hopefully make it a little more intelligible separating the concerns from the Provider/Consumer handshake.


#### S2 - Bolt11 parsing tools are important to provide to the app

The moneysocket libraries handles bolt11s for internal purposes, but the app typically will need to gaze into the bolt11s for the preimages and payment hashes for it's own accounting process. A nice, supported utility API for that kind of thing should be provided such that the apps don't need to re-implement.

#### S3 - Need to add a optional custom 'description' string on invoice request

Bolt11s have a description field, and for some uses, populating it will help add to the readability of the receipt trail. This should be an optional parameter to the `REQUEST_INVOICE` message.

#### S4 - Need to add an optional custom 'preimage' value on invoice request

For more advanced imagined uses like invoice chaining, a non-random preimage might be necessary. This should be an optional paramater to the `REQUEST_INVOICE` message.


#### S5 - Amountless invoices are not handled at all

This is a blind spot so far. Need to decide how to handle them. Either reject them outright give the app the abilty to fill in an amount.

`REQUEST_PAY` would need to have an optional parameter for msats that is mandatory for when requesting with amountless invoices.

#### S6 - Negative wad values

The accounting code is bit awkward needing to specify a positive/negative sign external to the Wad data structure. This could easily be accounted for inside the Wad data structure.

#### S7 - What is a 'receipt'?

This is kind of informal and done app-side to log events passing through an application. This is necessary to help debug, but it is also useful for transparency. Is there a reason to have the specification cover the various types of events and how to log them?


# Feature cloud talk

Stuff that might be nice to have eventually, but not at all viewed as a priority

#### C1 - OS domain sockets

There's a trend to bundle multiple aspects of a LN app into a single app executable (eg, node + UI + podcast player + messaging app, etc). An OS domain socket protocol like Moneysocket could help restore a more 'do one thing' approach to building desktop and mobile apps.

#### C-2 Bluetooth/NFC sockets

These short-range radio protocols are a way to get a pure P2P socket between two devices in physical proximity. This might be compelling for hardware projects where participants tether their phone with a local device or other phones to do an economic interaction using moneysocket. This shouldn't be too much of an effort to incorporate in the moneysocket stack, replacing websockets with other transports.


#### C-3 transaction receipt log updates

An account can be connected to multiple wallets during it's lifespan. The account will have a full receipt log, however, when disconnected, the wallet and upstream app won't get those updates. Does it make sense to have a 'synchronization' process to bring the events of an account up to the wallet?

#### C-4 onion routing

With the public key cystography layer, it will be possible to encrypt messages. however, for the message to reach it's destination, it might need to pass through several intermediaries. A standardized way to onion route the message might be advantageous. This would allow something comparable to Sphinx Chat to be built outside the Lightning Network, but with the same payment abilities and encryption.

#### C-5 What is the relationship between this and WebLN?

To the casual observer, Moneysocket, when running in a browser, looks like an alternative to a plugin. Moneysocket is that, but also a superset and not limited to just browser interactions. However, there is perhaps overlap between goals and desired features.

Also, WebLN might be a way to auto-connect a web app to a beacon-based rendezvous with the user's app. Particularly useful for sites where it's the user's first visit.

#### C-6 hosted channel-like invoicing

With the public-key crypto layers planned, each app will have significant ability to do cryptography themselves. This could include behaving more like a full lighting node at every stage, validating facts and generating one's own invoices over a Hosted Channel-style virtual presentation of the lightning network.

This would serve to optimize the minimization of trust.


#### C-7 spv-like validation across moneysocket chain

Like the above, this would be a way to propagate on-chain validation data through moneysocket to help minimize the trust necessary.
