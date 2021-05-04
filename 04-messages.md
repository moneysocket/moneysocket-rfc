# BOMS #4: Message Encoding

Messages for the wire are encoded in binary as a TLV stream, however the major part of the content is encoded as a JSON object TLV.

This design is to support experimentation and extensibility by allowing additional TLVs and/or fields to messages that can be ignored by implementations that don't understand the extension.

## Sender and Receiver

The application that encodes and sends the message is referred to as the Sender. Likewise, the application that receives and parses the message is referred to as the Receiver.

## Redundant information

Information in the TLVs is also redundantly incorporated in the JSON object. This allows the information to be accessed without the step of parsing the JSON. Also, for convenience of the protocol implementation, the JSON object can be considered alone to represent the message after parsing.

Additionally, since encryption wraps these messages, interpreting the TLVs in the stream are a straightforward check for whether the message has been decrypted to cleartext.

## Message as single TLV

A helpful feature to assist with parsing is to indicate a length of the data bytes that make up the entirety of a message. The TLV format is used to provide this length value with the contents of message embedded in the stream of TLVs.

1. type: 0 (`message`)
2. data:
    * [`tlv_stream`: `message_tlvs`]

### Message TLVs Data

The message data is encoded as a TLV stream

- MUST include a Sender Version TLV
- MUST include a Message Type TLV
- MUST include a JSON object TLV
- MAY include additional TLVs to pass non-standard data
    - SHOULD pick custom record `type` value following rules in [BOLT #01](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md#type-length-value-format).


#### Sender Version TLV

Specifies the version of Sender application.

- MUST set `major` version of the Moneysocket protocol of the Sender application
- SHOULD set `minor` version of the Sender application software release, otherwise set 0.
- SHOULD set `patch` version of the Sender application software release, otherwise set 0.

1. type: 0 (`version`)
2. data:
    * [`u8`: `major`]
    * [`u8`: `minor`]
    * [`u8`: `patch`]


#### Message Type TLV

Indicates the message type, either a request or notification and specifically which one.

- MUST indicate whether this message is a Request or Notification
    - `REQUEST`: `type` = `0x00`
    - `NOTIFICATION`: `type` = `0x01`
    - All other values are reserved.
- MUST indicate the enum value indicating specific message subtype
    - specific `subtype` enum values defined below
    - MAY indicate a custom `subtype` value
        - MUST pick a `subtype` value greater than 0x0000ffff
        - SHOULD pick random `subtype` values to avoid collisions with other custom subtype values


1. type: 1 (`message_type`)
2. data:
    * [`u8`: `type`]
    * [`bigsize`: `subtype`]

##### Request Message Enumeration Values

- `PAY`: `subtype` = `0x000000`
- `RENDEZVOUS`: `subtype` = `0x000001`
- `INVOICE`: `subtype` = `0x000002`
- `PING`: `subtype` = `0x000003`
- `PROVIDER`: `subtype` = `0x000004`
- `REQUEST`: `subtype` = `0x000005`

- All other values between zero and `0x0000ffff` are reserved

##### Notification Message Enumeration Values

- `ERROR`: `subtype` = `0x000000`
- `INVOICE`: `subtype` = `0x000001`
- `SEND_PREIMAGE`: `subtype` = `0x000002`
- `RECEIVE_PREIMAGE`: `subtype` = `0x000003`
- `PROVIDER`:  `subtype` = `0x000004`
- `PROVIDER_NOT_READY`: `subtype` = `0x000005`
- `RENDEZVOUS`:  `subtype`= `0x000006`
- `RENDEZVOUS_NOT_READY`: `subtype` = `0x000007`
- `RENDEZVOUS_END`: `subtype` = `0x000008`
- `PONG`:  `subtype` = `0x000009`

- All other values between zero and `0x0000ffff` are reserved


#### JSON object TLV
- MUST have appropriate contents in the language object
- MUST serialize language object into a JSON string
- MUST encode JSON string into a UTF-8-encoded byte stream

1. type: 2 (`json_object`)
2. data:
* [`u8`: `type`]
* [`len*byte`: `encoded_json`]


##### Language Object Message Contents

Language object discussed here as it is to be serialized into JSON. The specifics of how the object is manifested in language will vary by language. It is up to the implementation to produce JSON output that conforms.

- MUST include a name/value pair `"timestamp"` with a value of Number type.
    - MUST set `"timestamp"` value to the UNIX timestamp recorded at the time the message has been created by the sender.
    - MUST include a name/value pair `"version"` with a value of Object type
        - MUST have a length of 3, with the keys being `"major"` `"minor"` and `"patch"`.
        - MUST have a Number type for each of the 3 values
        - MUST set the '"major"` value to the `major` version of the Moneysocket protocol of the sender application
        - SHOULD set the '"minor"' value to the `minor` version of the sender application software release, otherwise set 0.
        - SHOULD set the `"patch"` to the `patch` version of the sender application software release, otherwise set 0.

    - MUST have a name/value pair `message_type` with a value of String type
        - MUST be value of either `"REQUEST"` or `"NOTIFICATION"`
    - MUST have a name/value pair `message_subtype` with a value of String type
        - MUST be value of the string representation of the enum value given in the Message Type TLV.
    - MAY have additional name/value pairs in the JSON as defined by the specific message and specific layer of the implementation.
    - MAY have additional non-specification name/value pairs in the JSON
        - MUST use distinct names to not conflict with name/value pairs used by the message subtype.
