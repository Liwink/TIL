## HTTP/1.1 Hop-by-hop Headers

##### End-to-end and Hop-by-hop Headers

[13.5.1](http://tools.ietf.org/html/rfc2616#section-13.5.1)

For the purpose of defining the behavior of caches and non-caching proxies, we divide HTTP deaders into two categories:

* End-to-end headers, which are transmitted to the ultimate recipient of a request or response. End-to-end headers in responses MUST be stored as part of a cache entry and MUST be transmitted in any response formed from from a cache entry.
* Hop-by-hop headers, which are meaningful only for a single transport-level connection, and are not stored by caches or forwarded by proxies.

The following HTTP/1.1 headers are hop-by-hop headers:

* Connection
* Keep-Alive
* Proxy-Authenticate
* Proxy-Authorization
* TE
* Trailers
* Transfer-Encoding
* Upgrade

##### [WebSocket proxying](http://tools.ietf.org/html/rfc2616#section-13.5.1)

> since the “Upgrade” is a hop-by-hop header, it is not passed from a client to proxied server. 
> With forward proxying, clients may use the CONNECT method to circumvent this issue. 
> This does not work with reverse proxying however, since clients are not aware of any proxy servers, and special processing on a proxy server is required.

> As noted above, hop-by-hop headers including “Upgrade” and “Connection” are not passed from a client to proxied server, therefore in order for the proxied server to know about the client’s intention to switch a protocol to WebSocket, these headers have to be passed explicitly.