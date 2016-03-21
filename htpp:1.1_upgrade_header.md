## HTPP/1.1 Upgrade header

The client need to make a clear-text request, which is later upgraded to a newer http protocol version or switched to a different protocol.

##### Use with WebSockets

WebSocket uses this mechanism to set up a connection with a HTTP server in a compatible way.

The WebSocket Protocol has two parts: a handshake to establish the upgraded connection, then the actual data transfer.

First, a client requests websocket connection by using the `Upgrade: websocket` and `Connection: Upgrade` headers, along with a few protocol-specific headers to establish the version being used and set up a handshake.
