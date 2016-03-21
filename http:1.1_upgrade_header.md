## HTTP/1.1 Upgrade header

The client need to make a clear-text request, which is later upgraded to a newer http protocol version or switched to a different protocol.

##### RFC 2616

[14.42 Upgrade](http://tools.ietf.org/html/rfc2616#section-14.42)

The Upgrade header field is intended to provide a simple mechanism for transition from HTTP/1.1 to some other, incompatible protocol.
Allow the client to advertise its *desire* to use another protocol, even though the current request has been made using HTTP/1.1, while it is determined by the server.

The Upgrade header field only applies to switching application-layer protocols upon the existing transport-layer connection.


##### Use with WebSockets

WebSocket uses this mechanism to set up a connection with a HTTP server in a compatible way.

The WebSocket Protocol has two parts: a handshake to establish the upgraded connection, then the actual data transfer.

First, a client requests websocket connection by using the `Upgrade: websocket` and `Connection: Upgrade` headers, along with a few protocol-specific headers to establish the version being used and set up a handshake.
The server, if it supports the protocol, replies with the same `Upgrade: websocket` and `Connection: Upgrade` headers and completes the handshake.
Once the handshake is completed successfully, data transfer bigins.
