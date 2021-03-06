# BOM #1: Encoding Primitives

## Primitives from BOLT

### Value Encoding

This specification borrows makes use of the Type-Length-Value Format for encoding data in byte streams specified in [BOLT #1](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md). This includes the [BigSize](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md#appendix-a-bigsize-test-vectors) type for compact unsigned `uint64` values.

Additionally, the [Fundamental Types](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md#fundamental-types) in BOLT #1, `byte`, `u16`, `u32`, `u64`, `tu16`, `tu32`, `tu64` and `sha256` encoding types are used in matching fashion.

### Invoice Encoding

For transacting on the underlying Lightning Network, Moneysocket handles [BOLT #11](https://github.com/lightningnetwork/lightning-rfc/blob/master/11-payment-encoding.md) strings and parses them to be aware of their contents as intended by their specification.


## Primitives from Bitcoin

### Bech32

Bech32 encoding format as specified in [BIP 0173](https://en.bitcoin.it/wiki/BIP_0173) is used by this specification and by BOLT 11 invoices.

### libsecp256k1

Public/Private key cryptography operations are performed on curve secp256k1, which is implemented in [libsecp256k1](https://github.com/bitcoin-core/secp256k1). It is highly recommended that this library underlie the execution of these crytographic operations.

### Schnorr Signatures

The signing of messages with Public/Private key pairs adheres to [BIP 0340](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki)

## Other standards

### JSON Object Encoding

When specifying objects meant to be held, manipulated and transmitted by an implementation, the language-independent terminology of JSON is used. When these objects are to be encoded into messages, [standard JSON](https://json.org) is used.

### UTF-8
When encoding JSON and other text into a byte stream, standard [UTF-8](https://www.utf8.com/) encoding used.

### UUIDv4
The protocol makes us of securely randomly-generated Universal Unique Identifiers. The standard string format of [Version 4](https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)) is exclusively used for this purpose.


## Convenience Types

#### ECDSA Signature
#### Schnorr Signature
#### ECDSA Signed Message
#### Schnorr Signed Message

#### AES Encrypted Message
