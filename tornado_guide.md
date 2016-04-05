
### Asynchronous and non-Blocking I/O

##### synchronous function

```
from tornado.httpclient import HTTPClient

def synchronous_fetch(url):
	http_client = HTTPClient()
	response = http_client.fetch(url)
	return response.body
```

##### asynchronous with a callback

```
from tornado.httpclient import AsyncHTTPClient

daf asynchronous_fetch(url, callback):
	http_client = AsyncHTTPClient()
	def handle_response(response):
		callback(response.body)		
	http_client.fetch(url, callback=handle_response)
```

##### asynchronous with a Future

```
from tornado.concurrent import Future

def async_fetch_future(url):
	http_client = AsyncHTTPClient()
	my_future = Future()
	fetch_future = http_client.fetch(url)
	fetch_future.add_done_callback(
		lambda f: my_future.set_result(f.result()))
	return my_future
```

`Futures` lend themselves well to use with coroutines.

```
from tornado import gen

@gen.coroutine
def fetch_coroutine(url):
	http_client = AsyncHTTPClient()
	response = yield http_client.fetch(url)
	return response.body
```

### Coroutines

**Coroutines** are the recommended way *to write asynchronous* code in Tornado.

##### How it works

> All generators are **asynchronous**; when called they return a generator object instead of running to completion.

```
class Handler():
	def __init__(self):
		super(Handler, self).__init__(*args, **kwargs)
		self.listen()
		print("Done listen.")

	@gen.coroutine
	def listen(self):
		yield tornado.gen.Task(self.client.subscribe, 'test_channel')
		print("Done task.")
		self.client.listen(self.callback)

>>> Done listen.
>>> Done task.
```

> The `@gen.coroutine` decorator communicates with the generator via the `yield` expressions, and with the coroutine's caller by returning a `Future`.

A simplified version of the coroutine decorator's inner loop:

```

	# Simplified inner loop of tornado.gen.Runner
	def run(self):
		# send(x) makes the current yield return x.
		# It returns when the next yield is reached.
		future = self.gen.send(self.next)
		def callback(f):
			self.next = f.result()
			self.run()
		futurn.add_done_callback(callback)
```

> The decorator receives a `Future` from the generator, waits(**without blocking**) for that `Future` to complete, then "unwraps" the `Future` and sends the result back into the generator as the result of the `yield` expression.

** ? Not found how to wait for futurn with blocking in source code. **
