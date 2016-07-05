## nginx

#### Worker Processes

nginx has *one master process* and *several worker processes*. The main purpose of the **master process is to read and evaluate configuration, and maintain worker processes**. **Worker processes do actual processing of requests**. nginx employs event-based model and OS-dependent mechanisms to efficiently distribute requests among worker processes. The worker processes is defined in the configuration file and may be fixed for a given configuration or automatically adjusted to the number of available CPU cores.

#### Configuration File's Structure

- simple directive:
- block directive: 

Directives placed in the configuration file **outside of any context** are consider to be in the **`main` context**.

The `events` and `http` directives reside in the `main` context, `server` in `http`, and `location` in `server`.



#### Setting Up a Simple Proxy Server

```nginx
server {
  listen 80;
  
  location / {
  	proxy_pass http:/localhost:8080
  }
}
```



#### directives

- `include` : includes another file, or files matching the specified mask, into configuration.

- `proxy_pass`: 

  >Syntax: **proxy_pass** URL;
  >
  >Default: -
  >
  >Context: location, if in location, limit_except

  Set the protocol and address of a proxied server and an optional URI to which a location should be mapped. As a protocol, "http" or "https" can be specified. The address can be specified as a domain name or IP address, and an optional port:
  `proxy_pass http://localhost:8000/uri/;`

  - if the `proxy_pass` directive is **specified with a URI**, the part of a normalized request URI matching the location is **replaced** by a URI specified in the directive.
  - if `proxy_pass` is specified without a URI, the request URI is passed  to the server in the **same form** as sent by a client when the original request is processed.
  - when the URI is changed inside a proxied location using the `rewrite` directive, in this case, the URI specified in the directive is ignored and the **full changed request URI** is passed to the server.

- `rewrite`:

  > Syntax: **rewrite** regex replacement [flag];
  >
  > Default: -
  >
  > Context: server, location, if

  If the specified regular expression matches a request URI, URI is changed as specified in the *replacement* string.
  An optional *flag* parameter can be one of:

  - break: stops processing the current set of  `ngx_http_rewrite_module` directives as with the break directive;
  - last: stops process the current set of `ngx_http_rewrite_module` directives and starts a search for a new location matching the changed URI;
  - redirect:
  - permanent:

- `break`:
  Stops processing the current set of `ngx_http_rewrite_module` directives. If a directive is specified inside the `location`, further processing of the request continues in this location.

  - `upstream`:

    > Syntax: **upstream** name { … }
    >
    > Context: http

    Defines **a group of servers**. Servers can listen on *different ports*. In addition, servers *listening on TCP and UNIX-domain sockets* can be mixed.

    ```nginx
    upstream backend {
      server backend.example.com weight=5;
      server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
      server unix:/tem/backend;
      server backup.example.com backup;
    }
    ```

    ​