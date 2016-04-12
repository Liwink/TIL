## I/O

##### stream

##### 非阻塞忙轮询 I/O

```
while True:
	for i in streams[]:
		if i has data:
			read until unavailable
```

##### select 阻塞当前线程

```
while True:
	# block
	select(streams[])
	for i in streams[]:
		if i has data:
			read until unavailable
```

##### epoll 

```
while True:
	# block
	active_streams[] = epoll_wait(epollfd)
	for i in active_streams[]:
		if i has data:
			read until unavailable
```

