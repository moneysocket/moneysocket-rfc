# BOM #6: Rendezvous layer

The Rendezvous layer performs an application's part in establishing a communications channel with another application.

## Roles
An instance of the Rendezvous layer is assigned a role of either a `Matchmaker` or a `Single`.

A `Matchmaker` will perform the duty of pairing two `Single` connections into a rendezvous relationship. Typically, a relay or other app that is listening incoming connections will take the `Matchmaker` role.

A `Single` will connect to a `Matchmaker` and allow the `Matchmaker` to complete the rendezvous to another `Single` that provides the same `Rendezvous ID` value.

## Values

### Shared Seed

The 128-bit `Shared Seed` value is exchanged in the beacon between two applications seeking rendezvous.

### Rendezvous ID

The `Rendezvous ID` value is derived from the 128-bit `Shared Seed` value encoded in the beacon.

The `Rendezvous ID` value is the 256-bit value that is the result of the 2nd round of SHA256 applied to the 128-bit `Shared Seed` value. The 1st round of SHA256 applied to the 128-bit shared seed value is the `AES Encryption Key`. Hence, the `Rendezvous ID` is also the result of applying SHA256 result to the `AES Encryption Key`.

### AES Encryption Key

All post-rendezvous content passed through the layer and via connection will be encrypted with a layer of AES content. This ensures that anyone who has not observed the beacon cannot decrypt the content. This 256-bit value is the result of a SHA256 operation on the 128-bit `Shared Seed` value.


### Tiebreaker

The `Tiebreaker` is a 64-bit value to be formatted as a hexadecimal string. Its function is to provide a way to automatically resolve which application among the two at the ends of a connection takes on the `Matchmaker` role in the case of a conflict.

In the case of a conflict during the handshake, the application with the highest numerical value will be assigned the role of `Matchmaker`.

For automatic resolution, an application should select a randomly-generated value. To insist on a `Matchmaker` role, an application should select this value to be `"ffffffffffffffff"`. To defer to the other side (but still allow oneself to assume the `Matchmaker` role when confronted with a `Single`), an application should select this value to be `"0000000000000000"`.


## Messages

Messages are encoded as defined in [04-messages.md](BOM #4).

### REQUEST\_RENDEZVOUS

The TLV data
- MUST set `message_type` value to `0x00`
- MUST set `message_subtype` value to `0x01`

The Language Object:

- MUST set `message_type` String-typed value to `REQUEST`
- MUST set `message_subtype` String-typed value to `RENDEZVOUS`
- MUST have a name/value pair `role` with a value of String type
    - MUST be set to either `MATCHMAKER` or `SINGLE` to indicate the application's role

- if `role` is set to `SINGLE`
    - MUST have a name/value pair `rendezvous_id` with a value of String type.
        - MUST be set to a hex-formatted string of 256-bit `rendezvous_id` value

- if `role` is set to `MATCHMAKER`
    - MUST have a name/value pair `tiebreaker` with a value of String type
        - MUST be randomly-generated or specifically-chosen according to desired Tiebreaking behavior
    - MUST have a name/value pair `rendezvous_id` with a value of String type.
        - MUST be set to a hex-formatted string of 256-bit `rendezvous_id` value


### NOTIFY\_RENDEZVOUS
### NOTIFY\_RENDEZVOUS\_NOT\_READY
### NOTIFY\_RENDEZVOUS\_END

## Behavior

- Upon establishing a connection, whether outgoing or incoming
    - MUST send a REQUEST\_RENDEZVOUS message.
    - MUST set `role` to be `MATCHMAKER` or `SINGLE`




## Appendix A - Shared Seed Derivation Test Vectors

