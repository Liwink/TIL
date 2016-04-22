## 由Lua 粘合的Nginx生态环境

##### 2.1 OpenResty
> 对看起来是整体的 Web 应用，习惯在后台拆分成很多 Service，有些 Service 是供给客户端发起请求来访问的，而有些 Service 根本就是为其他服务而服务的，也使用了 HTTP 协议进行发布。

HTTP - Nginx

（似乎很多创业公司也习惯将业务写在一起）

为什么 Nginx 快

* 单线程
* 事件驱动，只有相关事件发生时，才处理具体数据，否则切换到其它请求
* 很容易将CPU跑满（而 Apache 多线程更占内存）

OpenResty

* Nginx 补丁
* 新增模块
* 将 Lua 和常用的库嵌入 Nginx

##### 配置小语言


