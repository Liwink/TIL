##### Nginx 转发

Nginx 代理转发时保留原 url

```nginx
location / {
    proxy_pass http://kwapi_demo.backend;
    proxy_set_header Host $host;
    break;
}
```

[How to preserve request url with nginx proxy_pass](http://stackoverflow.com/a/5834665/3873366)