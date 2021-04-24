This is a capture of a bunch of stuff that is known to be deficient in the pre-specification implementations.

# Big ommisions

A couple things that are missing from the implementatation that are very desirable features.

Feature-creep is a problem, but these things might be necessary.

## Public-key crypto layer

- Replay attacks from the relay are probably possible
- man-in-the-middle attacks are easier than they need to be
- Cryptographic identity is missing (useful for Nostr, Moneysocket Chat, lnurl-auth style applications etc.)
- Encryption of messages (Nostr DMs, other chat systems).

Diffie-Hellman exchange
ProviderInfo announces key set and types of signatures that can be performed.
Layer of block cypher encryption
Sign message call
Encrypt call

## WebRTC

There is a criticism that the websocket-based moneysocket connections require the relay to work best. This adds latency and a tendancy to depend on centralized hosted infrastrcture.

When performing a 'rendezvous', sometimes we can have endpoints perma-listen, waiting for the other side to join, but other times we need to manually send in each side to meet.

With this relay scheme, it feels like we are just re-inventing WebRTC badly. The desired abstraction is like a peer-to-peer 'phone call' to take a direct route from point-to-point. This is an hint/indication that we should use literal telephony techonlogy to establish the socket.


# Medium-sized issues

## Provider/Consumer Transact Layer is poorly named

There's Provider/Consumer layer and then Provider/Consumer 'Transact' layer that are confusing to have side-by-side. Suggest "API" layer as a naming basis. "API Provider Layer" & "API Consumer Layer".

The API provider layer consumes services from the app to provide the API to the remote API consumer layer. The API consumer layer calls into the API to orchestrate payments

## Beacon rendezvous setup is error-prone.

There's a directionality/gender to the beacon connection that's not obvious. Common to some standards/protocols (USB 2.0, ethernet crossover, LNURL (SERVICE vs WALLET), etc.)

At the very least, need to produce a descriptive error message when connecting th wrong way.
Perhaps hint in the beacon what 'gender' it's expecting to join
Perhaps put a symbol for the 'gender' in the QR code

## KeySend is desired

For the [value4value podcast index tags](https://github.com/Podcastindex-org/podcast-namespace/blob/main/value/value.md), they specify a node endpoint for you to KeySend value. This scheme has controversy since bolt11s provide proof-of-payment and the KeySend scheme does not (this is confusing for both sender and receiver about 'why' the payment is happening). However, it does have some attraction and providing KeySend as a moneysocket request isn't a monumental amount of effort.

This would at least allow us to implement a demo `value4value` application that conforms to this standard.

## Announcing nexus to applicaiton is not coupled with ProviderInfo

ProviderInfo comes in a different call later, so a nexus isn't fully online and useful to the app upon being announced. Fixing this would make the library API simpler.

## Accounting for routing fees not done well

`NOTIFY_PREIMAGE` is sent back both after an outgoing and incoming payment.

For outgoing, it comes back with what the routing fee was, but that's not propagated upstream for the user to use.

Also, the user might want to have some control over what is an acceptable routing fee to pay for an outgoing payment.

## Not entirely sure version numbering is a good way to go

## Message format and versioning scheme could be better tuned for extensibility

-

## re-using SharedSeed values leads to undefined behavior

## re-using Preimage values leads to undefined behavior

- re-using values might be necessary/interesting for some chained appliations.


# Small-ish issues

## Bolt11 parsing tools are important to provide to the app
The moneysocket library handle bolt11

## Need to add a optional custom 'description' string on invoice request

## Need to add an optional custom 'preimage' value on invoice request



# Feature cloud talk

Stuff that might be nice to have eventually, but not at all viewed as a priority

## hosted channel-like invoicing
## transaction receipt log updates
## onion routing
## spv-like validation across chain



