# Protocol Stack, Layers and Nexuses

What we call a `stack` is a [protocol stack](https://en.wikipedia.org/wiki/Protocol_stack) that uses a layered architecture pattern. Such a pattern is typical and common practice for implementing network protocols.

It is not mandatory that an Moneysocket implementation adhere to this software architecture. However, this specification uses the concept and terminology to describe the various pieces of the protocol.


## Stack

The stack is composed of individual layers that can be envisioned as arranged vertically, with the transport layers (eg. a WebsScket connection interface) at the bottom and the API layers connecting to an application at the top. The middle layers perform the protocol handshakes to establish the context of the socket communication session.

The layer interfaces with the application via software API calls. A stack instance provides an API an for the application to call to drive actions. The stack also requires input from the application to obtain data and settings for protocol behavior, typically provided by registered function calls.

In Moneysocket, because it is an asymmetric protocol, there are two types of stacks - one for the Provider role and one for the Consumer role.

## Layers

Each layer of the stack takes on a sub-responsibility of the protocol. It has a 'downwards'-facing interface to the layer below. The layer below provides nexuses for the layer to interact with. It has an 'upwards'-facing interface to the above layer. The layer passes produced nexuses to the above layer for it to interact via the above layer's downwards-facing interfacing.

At the bottom of the stack, the layer will interface with a transport service that provides a raw communication socket.

At the top of the stack, the layer will interface with the stack instance to help provide an external API for the stack.

### Upwards-facing interface

#### `on_announce` callback

The layer calls this to inform the above layer of a new nexus. Nexus object is passed as a parameter.

#### `on_revoke` callback

The layer calls this to inform the above layer of a nexus being revoked. An identifier for the nexus is passed as a parameter so the above layer can delete it's reference.

### Downwards-facing interface

#### Registering `on_announce` call-in

The above layer will register with a below layer to be informed of a new nexus announced by the below layer.

#### Registering `on_revoke` call-in

The above layer will register with a below layer to be informed of a nexus being revoked by the below layer.

## Nexus

A `nexus` is an internal abstraction of a socket object. It is created by a layer and typically wraps one or more nexus produced by the layer below. It provides an API to drive actions and forward received messages upwards. Typically, to execute actions, it will call the API of the nexus it wraps. If it receives a message, it may handle the message, forward the message, or process the message before forwarding upwards.

### API interface

The specific nexus subtype may define additional actions

#### `send` call

A call to send a message that is in the form of a language object.

#### `send_bin` call

A call to send a binary message that may be cyphertext.

#### `get_shared_seed` call

TODO - should this be rendezvous id?

#### `on_message` callback

A callback registered on the nexus for a wrapping component to receive a message is in the form of a language object.

#### `on_bin_message` callback

A callback registered on the nexus for a wrapping component to receive a binary message that may be cyphertext.

