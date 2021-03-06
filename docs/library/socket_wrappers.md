# `socket_wrappers.hpp`

The files defines simple wrappers of socket programming in `Networking::SocketWrappers` namespace.

## `PortType` type definition

The alias of `uint16_t`. Represents a port number.

## `SocketInitialiser` structure

This is a scoped socket initialiser-unintialiser guard.

Members:

- Constrcutor: tries to initialise socket programming environment.
- Destructor: tries to deinitialise socket programming environment.
- `bool Good() const` function: indicates whether the initialisation was successful.

## `SocketConsumer` structure

Consumes a socket for sending, receiving and skipping bytes.

Members:

- Constructor `(SOCKET target)`: constructs the consumer with `target` as the socket.
- `bool Send(size_t sz, void const *buf) const`: sends the data of size `sz` pointed to by `buf`, returning whether the operation was successful.
- `bool Receive(size_t sz, void *buf) const`: receives data of size `sz` and stores them in the region `buf`, returning whether the operation was successful. The call does not return until an error has occurred or the data of size `sz` have been received.
- `bool Skip(size_t sz) const`: ignores data of size `sz`. The call does not return until an error has occurred or the data of size `sz` have been ignore. On Windows, the implementation simply copies and discards the data.

## `SocketUnique` structure

A scoped socket handle holder. It closes the socket it owns (if there is one) upon destruction.

Members:

- Default constructor: lets the object own no socket.
- Constructor `(SOCKET target)`: lets the object own `target`. If `target` is `INVALID_SOCKET`, the object does not own a socket.
- Move constructor: takes the ownership from the other object.
- Move assignment: disposes the current object and takes the ownership from the other object.
- Destructor: closes the socket it owns (if any).
- `bool IsValid() const` function: returns whether the object owns a socket or not.
- `SOCKET RawValue() const` function: returns the `SOCKET` the object owns. If the object owns no socket, `INVALID_SOCKET` is returned. Calling this function does **not** invalidate the ownership.
- `SOCKET RevokeOwnership()` function: lets the object own no socket and returns the `SOCKET` the object previously owned.
- `void Dispose()` function: disposes the `SOCKET` the object owns (if any).

## `ServerConnectToClient` function

The formal parameter `port` is a `PortType` that represents the port on which the server listens. The function creates a server socket, waits for the first client that connects to it, stops listening and returns the socket used to communicate with the connected client.

## `ClientConnectToServer` function

The formal parameters are:

- `server`: a `char cosnt *`, the IPv4 address of the server.
- `port`: a `PortType`, the port to which the client should connect.

The function tries to connect to the specified port of the specified server and returns the socket used to communicate with the connected server.
