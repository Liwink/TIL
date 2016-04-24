## OpenResty Get Started

##### install 

类似[这里](http://homeway.me/2015/07/10/rebuild-osx-environment/)遇到的问题，装 OpenResty 用了很久。
主要是 OpenSSL 和 PCRE 路径和版本问题。
解决方法就是指点对应路径，注意需要是 source 文件。

`sudo ./configure --with-openssl=../openssl-1.0.2g --with-pcre=../pcre-8.38`

我提前将 source 下载到相对路径上。
