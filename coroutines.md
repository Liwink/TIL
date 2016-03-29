## Coroutines

* Despite some similarities, generators and coroutines are basically two different concepts
* Generators **produce** values
* Coroutines tend to **consume** values

##### Pipeline

```
import time
def follow(thefile, target):
	thefile.seek(0, 2)
	while True:
		line = thefile.readline()
		if not line:
			time.sleep(0.1)
			continue
		target.send(line)
```

```
@coroutine
def grep(pattern, target):
	while True:
		line = (yield)
		if pattern in line:
			target.send(line)
```

```
@coroutine
def printer():
	while True:
		line = (yield)
		print(line)
```

```
f = open("access-log")
follow(f,
	   grep("python",
	   		printer())))
```

##### A Threaded Target

