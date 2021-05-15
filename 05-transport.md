# BOM #6: Transport layer

The Transport layer has the duty of adapting the layered architecture to the underlying socket transport. The specific mechanism of the underlying transport is not limited by this specification.

## Values

### Shared Seed

The `Shared Seed` value must be available in order to establish a connection. A round of SHA256 can be applied to establish the AES encryption key

### AES Encryption Key


## API to above layer

### Send Message

Layer above can call to send a message over the transport where the message is passed as a language object. The transport layer will format it into a byte stream representation and may encrypt it.

### Send Bin Message
Layer above can call to send a message over the transport where the message is passed as a byte stream. The transport layer will apply a layer of encryption.


### Get Shared Seed

The `connect()` call will bind a shared seed value to the created connection and associated transport nexus. For rendezvous and user interface functions, this shared seed needs to be accessed and used as a reference to this particular connection. Hence an API call to retreive it from this layer is appropriate.


## Messages

## REQUEST\_TRANSPORT\_PING

- This message is exempt from transport layer encryption

The TLV data
- MUST set `message_type` value to `0x00`
- MUST set `message_subtype` value to `0x08`

The Language Object:

- MUST set `message_type` String-typed value to `REQUEST`
- MUST set `message_subtype` String-typed value to `TRANSPORT_PING`
- MUST set a `request_uuid`

## NOTIFY\_TRANSPORT\_PONG

- This message is exempt from transport layer encryption

The TLV data
- MUST set `message_type` value to `0x01`
- MUST set `message_subtype` value to `0x08`

The Language Object:

- MUST set `message_type` String-typed value to `REQUEST`
- MUST set `message_subtype` String-typed value to `TRANSPORT_PONG`
- MUST set a `request_reference_uuid` to be the `request_uuid` of the

## Behavior

### Consumer/Provider

Typically consumers send `REQUEST` messages, and `Providers` reply with `NOTIFICATION`, however at this level of the stack the applications have yet to differentiate into Consumer/Provider roles and either type of message is permitted.

### Connnect/Disconnect

Upon establishing a socket connection
- MUST announce a nexus to the above layer

Upon the transport closing a connection
- MUST revoke a nexus from the above layer

### Message Encryption

Messages that are passed from above to be sent:
    - If messages are passed in cyphertext from the above layer:
        - MUST be encrypted with AES256 using the `AES Encryption Key` value derived from teh `Shared Seed`
    - If messages are passed in cleartext from the above layer:
        - If message type and subtype is categorized as exempt from encryption (Rendezvous layer handshake messages)
            - MUST pass to transport unencrypted
        - If not exempt:
            - MUST be encrypted with AES256 using the `AES Encryption Key` value derived from teh `Shared Seed`

Messages that are recieved from below sent:

- If they are cyphertext
    -> MUST decrypt with `AES Encryption Key`

- If they are cleartext
    -> MUST check that message is exempt from encryption
    -> MUST discard all non-exempt messages

### Ping/Pong

For some connection transports (such as websockets and other internet-spanning) , inactive connections are often closed by network equipment that is monitoring

- MAY send a `TRANSPORT_PING` at any time from any side of the connection
- MUST send a `TRANSPORT_PONG` in reply to any received `TRANSPORT_PING`
- if PING is not responded to by PONG within a reasonable timeframe:
    - MUST close the connection
- SHOULD NOT have more than one `TRANSPORT_PING` outstanding at one time
- SHOULD NOT send more than one `TRANSPORT_PING` per second


## Appendix A - AES Encryption Test Vectors
