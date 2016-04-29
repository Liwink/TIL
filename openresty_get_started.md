## OpenResty Get Started

##### install 

类似[这里](http://homeway.me/2015/07/10/rebuild-osx-environment/)遇到的问题，装 OpenResty 用了很久。
主要是 OpenSSL 和 PCRE 路径和版本问题。
解决方法就是指点对应路径，注意需要是 source 文件。

`sudo ./configure --with-openssl=../openssl-1.0.2g --with-pcre=../pcre-8.38`

我提前将 source 下载到相对路径上。

##### hello world

配置 Nginx 路径 `PATH=/usr/local/openresty/nginx/sbin:$PATH`

##### location

内部调用：

* `internal` 只允许内部调用
* `capture_multi` 并行、非阻塞执行两个子请求

##### body

> 由于 Nginx 是为了解决负载均衡场景诞生的，所以它默认是不读取 body 的行为，会对 API Server 和 Web Application 场景造成一些影响。根据需要正确读取、丢弃 body 对 OpenResty 开发是至关重要的。