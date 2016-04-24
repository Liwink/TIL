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

nginx.conf 配置文件，本身其实就是一个小语言
(The Nginx *configure file* notation is a small **language**)

```
location = "/hello" {
	set_unescape_uri $person $arg_person;
	set_if_empty $person "anonymous";
	echo "Hello, $person!";
	}
```

* 使用类似正则表达式的形式来约定响应 url
* 使用 Nginx 指令对内部变量进行操作
* 开发者都使用 Nginx 的开发接口，丰富配置文件小语言的词汇表 "vocabulary"
* ngx_memc: an Nginx **upstream** module for Memocached

上游、下游

* 其后各种服务，比如 redis，在 Nginx 而言就是上游
* 而访问 Nginx 的客户端都视为下游

##### ngx_memc

```
location = /memc {
	set $memc_cmd $arg_cmd;
	set $memc_key $arg_key;
	set $memc_value $arg_val;
	set $memc_exptime $arg_exptime;
	memc_pass 127.0.0.1:11211;
	}
```

* 通过 `set` 将 url 上几个参数映射到对应变量
* 通过 `memc_pass` 连接到远端的 memcached 服务，当然后面可以是个集群
* 于是得到一种类似 restful 的 memcached 接口服务，可以使用 `curl` 来操作 （是如何将参数传递给 memc？）

HTTP 封装业务

* 不论业务由什么语言实现，包括 MySQL 业务集群，都可以包装成 HTTP 接口
* 统一 HTTP 接口，系统复杂性降低
* HTTP 协议本身简单，工具链丰富
* 淘宝开放平台也通过开放平台，将内部服务，封装为一系列 HTTP 接口


##### ngx_drizzle

非阻塞通信

```
upstream my_mysql_backend {
	drizzle_server 127.0.0.1:3306 dbname=test
		password=pwd user=mon protocol=mysql;
	drizzle_keepalive max=200 overflow=reject;
	}
```

```
location ~ '^/cat/(.*) {
	set $name $1;
	set_quote_sql_str $quoted_name $name;
	drizzle_query "select *
		from cats
		where name=$quoted_name";
	drizzle_pass my_mysql_backend;
	rds_json on;
```

##### 嵌入 Lua

```
location = /hello {
	content_by_lua '
		ngx.say("Hello World")
	';
}
```

* `ngx.say` 是 Lua 显露给模块的接口
* `content_by_lua_file` 调用外部文件

Ngnix 子请求

```
location = /memc {
	internal;
	memc_pass ...;
	}

location = /api {
	content_by_lua '
		local resp = ngx.location.capture("/memc")
		if resp.status ~= 200 then
			ngx.exit(500)
		end
		ngx.say(resp.body)
	';
	}
```

* `ngx.location.capture` 发起一个 location 请求，但是没有 HTTP 的开销，因为这是 C 级别的内部调用 （？并没有走 HTTP？）
* 而且这是一个异步调用，虽然我们是以同步的方式来写的

##### 同步形式异步执行

* JavaScript 中实现异步效果需要使用回调
* Lua 中可以这样是因为 Lua 支持协程，即 concurrent
* 这样就可以在一个 Lua 线程中分割出多个 Lua 用户级的逻辑线程
* （Tornado 也是这样实现「同步形式异步执行」，这里为什么不用 async 和 await？Tornado 具体实现又是什么。tornado-redis 又是如何支持非阻塞的？）

##### lua_shared_dict

* Nginx 跑多 worker 模型，使用多进程，主要是为了跑满 CPU （worker ？）
* 多进程间共享内存，红黑树 + 自旋锁 实现：
	* 红黑树的查找类似 hash 表查找的一种算法
	* 为保持读写的数据一致性，使用自旋锁来保证

```
lua_shared_dict dogs 10m;

server {
	location = /set{
		content_by_lua '
			local dogs = ngx.shared.dogs
			dogs:set("Tom", ngx.var.arg_n)
			ngx.say("OK")
		'
	}
	
	location = /get{
		content_by_lua '
			local dogs = ngx.shared.dogs
			ngx.say("Tom: ", dogs.get("Tom"))
		';
	}
}
```

##### 同步非阻塞的 socket 接口

* 可以让 Lua 直接通过 HTTP 或 Unix socket 协议访问任意后端服务（不然是怎么访问？通过 Nginx 上游模块？）
* 可以进行非阻塞的访问，当超时时，Nginx 自动挂起，切入其它协程进行处理（协程中任务切换是如何进行？可以看Tornado是如何做的）
* 如果所有连接都不活跃，就等系统的 epoll 调用（IOLoop 和协程的关系）

```
local sock = ngx.socket.tcp()

sock:settimeout(1000) -- one second

local ok, err = sock:connect("127.0.0.1", 11211)
if not ok then
	ngx.say("failed to connect: ", err)
	return
end
```






