# Consumers and Providers

Moneysocket is a asymmetrical protocol. One side of the connection is a Provider and provides API services. The other side of the connection is a Consumer and consumes the API services.

There must be a Consumer and a Provider in a connection. A Consumer cannot connect to a Consumer and a Provider cannot connect to a Provider.

## Applications

The role is a property of the connection and not the application as a whole. However, in discussion the role might be referred to as a property of an application.

An application can have multiple connections, each of which can be either role. If it makes sense for a particular composed system, an application may have two connections open to another application with opposite role arrangements within each connection.

## Automatic role

A connection may choose to be late-binding into one of these two roles. In which case, it will be referred to as having an "Automatic" role, however it will assume either a Consumer or a Provider role upon completing a connection, provided it can find the right role to partner with.

The application's connection will choose a role depending on the role of the other side of the connection as discovered via the handshake.

