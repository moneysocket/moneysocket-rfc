# BOM #9: Roles

Monysocket is an asymmetric protocol where the application at one end of the socket connection is a Provider and the application at the other end of the connection is a Consumer.

The Role Layer performs the handshake to pair an application Consumer with a Provider. The handshake also allows an application to 'automatically' choose a role based on the role present at the other end of the connection.

The purpose of this layer is limited to determinining which application at the two ends is the Consumer and which is the Provider.


## Messages

Messages are encoded as defined in [04-messages.md](BOM #4).

## REQUEST\_ROLE

The TLV data
- MUST set `message_type` value to `0x00`
- MUST set `message_subtype` value to `0x05`

The Language Object:

- MUST set `message_type` String-typed value to `REQUEST`
- MUST set `message_subtype` String-typed value to `ROLE`

- MUST have a name/value pair `requester_role` with a value of String type
    - MUST be set to either `PROVIDER`, `CONSUMER`, or `AUTOMATIC` to indicate the sending application's role

- if `requester_role` is set to `AUTOMATIC`
    - MUST have a name/value pair `tiebreaker` with a value of String type
        - MUST be randomly-generated or specifically-chosen according to desired tiebreaking behavior
            - if automatic tiebreaking desired:
                - MUST set to hex-formatted random 64-bit value
            - if self being assigned `Provider` tiebreaking desired:
                - MUST set to `"ffffffffffffffff"`
            - if self being assigned `Consumer` tiebreaking desired:
                - MUST set to `"0000000000000000"`

## NOTIFY\_ROLE

The TLV data
- MUST set `message_type` value to `0x01`
- MUST set `message_subtype` value to `0x05`

The Language Object:

- MUST set `message_type` String-typed value to `NOTIFY`
- MUST set `message_subtype` String-typed value to `ROLE`

- MUST have a name/value pair `notifier_role` with a value of String type
    - MUST be set to either `PROVIDER`, `CONSUMER`


## NOTIFY\_ROLE\_END


## Behavior

### Handhsake

- Upon receiving a nexus announcement from layer below:
    - MUST send a `REQUEST_ROLE` message
    - MUST set `requester_role` to the role of the local application
    - if `CONSUMER` role:
    - if `PROVIDER` role:
    - if `AUTOMATIC` role:


- Upon receiving a `REQUEST_ROLE` message from the below layer
    - if local role is `CONSUMER`:
        - if `requester_role` in message is `CONSUMER`:
            - MUST send error
            - MUST disconnect within 1 second
        - if `requester_role` in message is `PROVIDER`:
            - MUST send `NOTIFY_ROLE`
            - MUST assign `consumer` role to nexus
            - MUST set `notifier_role` to `CONSUMER`
            - MUST announce nexus to layer above
        - if `requester_role` in message is `AUTOMATIC`:
            - MUST send `NOTIFY_ROLE`
            - MUST set `notifier_role` to `CONSUMER`
            - MUST announce nexus to layer above

    - if local role is `PROVIDER`:
        - if `requester_role` in message is `CONSUMER`:
            - MUST send `NOTIFY_ROLE`
            - MUST set `notifier_role` to `PROVIDER`
            - MUST assign `provider` role to nexus
            - MUST announce nexus to layer above
        - if `requester_role` in message is `PROVIDER`:
            - MUST send error
            - MUST disconnect within 1 second
        - if `requester_role` in message is `AUTOMATIC`:
            - MUST send `NOTIFY_ROLE`
            - MUST set `notifier_role` to `PROVIDER`
            - MUST assign `provider` role to nexus
            - MUST announce nexus to layer above

    - if local role is `AUTOMATIC`:
        - if `requester_role` in message is `CONSUMER`:
            - MUST send `NOTIFY_ROLE`
            - MUST set `notifier_role` to `PROVIDER`
            - MUST assign `provider` role to nexus
            - MUST announce nexus to layer above
        - if `requester_role` in message is `PROVIDER`:
            - MUST send `NOTIFY_ROLE`
            - MUST set `notifier_role` to `PROVIDER`
            - MUST assign `provider` role to nexus
            - MUST announce nexus to layer above
        - if `requester_role` in message is `AUTOMATIC`:
            - if `tiebreaker` evaluates to local role to be `PROVIDER`:
                - MUST send `NOTIFY_ROLE`
                - MUST set `notifier_role` to `PROVIDER`
                - MUST assign `provider` role to nexus
                - MUST announce nexus to layer above
            - if `tiebreaker` evaluates to local role to be `CONSUMER`:
                - MUST send `NOTIFY_ROLE`
                - MUST set `notifier_role` to `PROVIDER`
                - MUST assign `provider` role to nexus
                - MUST announce nexus to layer above
            - if `tiebreaker` cannot be evaluated due to a value collision:
                - MUST send error
                - MUST disconnect within 1 second

- Upon receiving a `NOTIFY_ROLE_END` message from the below layer
    - MUST revoke nexus from above layer
    - MUST disconnect within 1 second

