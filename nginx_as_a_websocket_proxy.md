## NGINX as a WebSocket Proxy

##### Supported Version

NGINX has supported WebSocket since NGINX 1.3 and can act as a reverse proxy and do load balancing of WebSocket applications.

##### WebSocket and HTTP

The WebSocket *handshake* is compatible with HTTP, using the **HTTP Upgrade** facility to upgrade the connection from HTTP to WebSocket.


##### NGINX challenges

> One is that WebSocket is a **hop-by-hop** protocol, so when a proxy server intercepts an Upgrade request from a client it needs to send its own Upgrade request to the backend server, including the appropriate headers.
>  Also, since WebSocket connections are long lived, as opposed to the typical short-lived connections used by HTTP, the reverse proxy needs to allow these connections to remain open, rather than closing them because they seem to be idle.

##### NGINX config

The `Upgrade` and `Connection` headers must be set explicitly.

```
location /wsapp/ {
	proxy_pass http://wsbackend;
	proxy_http_version 1.1;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection "upgrade";
}
```