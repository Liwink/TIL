## Iterators and Generators

##### yield 返回 generator

```
def test_yield():
    return
    if False:
        yield
```

```
>>> type(test_yield)   # function
>>> type(test_yield()) # generator
```

1. 识别到函数中有 `yield` 关键字，就返回 `generator`
2. `yield` 会让 `generator` 挂起它的执行

##### next 后执行顺序

```
def iterator():
    print('start')
    n = 3
    while n > 0:
        print('before: ', n)
        yield n 
        print('after: ', n)
        n -= 1
    print('end')
    while True:
        time.sleep(5)
```


```
>>> i = iter(iterator())
>>> next(i)
```

1. `next` 会驱动 generator 开始运行
2. 一次 `next` generator 会停在 `yield` 处
3. generator 没有return时，`next` 会阻塞（？似乎是非阻塞的）

##### send 

```
def test_send():
    while True:
        print('before yield')
        text = yield
        print('after yield')
        print(text)
```

```
>>> i = test_send()
>>> next(i)        # before yield
>>> i.send('test') # after yield; test; before yield
>>> next(i)        # after yield; None; before yield
```

1. `yield` 是非阻塞的！
2. 没 `send` 时 `yield` 会将 `None` 推出
3. `send` 会促发 `__next__()`，并传递值
4. `send(None)` 可以启动协程

```
def test_send():
    while True:
        print('before yield')
        text = yield 1
        print('after yield')
        print(text)
```

```
>>> i = test_send()
>>> next(i)        # before yield; [Output] 1
>>> i.send('test') # after yield; test; before yield; [Output] test
>>> next(i)        # after yield; None; before yield; [Output] 1
```

1. `text = yield 1` 这段有点怪

##### 阻塞回调

```

	def framework(logic, callback):
		s = logic()
		print('[FX] logic: ', s)
		print('[FX] do someting')
		callback('async: ' + s)

	def logic():
		s = 'mylogic'
		return s

	def callback(s):
		print(s)
		
	>>> framework(logic, callback)
```

##### 非阻塞回调

```

	def framework(logic):
		try:
			it = logic()
			s = next(it)
			print("[FX] logic: ", s)
			print("[FX] do someting")
			it.send("async: " + s)
		except StopIteration:
			pass
	
	def logic():
		s = "mylogic"
		r = yield s
		print(r)
```

但上面并不能看出「非阻塞」和「阻塞」式的区别！
写出一个看得出区别的例子！
