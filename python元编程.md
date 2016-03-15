## 元编程

### 在函数上添加包装器

```
import time
from functools import wraps

def timethis(func):
	@wrap(func)
	def wrapper(*args, **kwargs):
		start = time.time()
		result = func(*args, **kwargs)
		end = time.time()
		print(func.__name__, end-start)
		return result
	return wrapper
```

下面两种写法的效果相同

```
@timethis
def countdown(n):
	pass
```

```
def countdown(n):
	pass
countdown = timethis(countdown)
```

### 创建装饰器时保留函数元信息

`@wraps`可以使被装饰函数保留有用信息，如`__name__`, `__doc__`, `__annotations__`

###解除一个装饰器

```
countdown.__wrapped__(1)
```

### 定义一个带参数的装饰器

```

	from functools import wraps
	import logging

	def logged(level, name=None, message=None):
		def decorate(func):
			logname = name if name else func.__module__
			log = logging.getLogger(logname)
			logmsg = message if message else func.__name__
		
			@wraps(func)
			def wrapper(*args, **kwargs):
				log.log(level, logmsg)
				return func(*args, **kwargs)
			return wrapper
		return decorate

	@logged(logging.DEBUG)
	def add(x, y):
		return x + y

```

下面两种写法等效：

```
@decorator(x, y, z)
def func(a, b):
	pass
```

```
def func(a, b):
	pass
func = decorator(x, y, z)(func)
```

### 可自定义属性的装饰器

##### 问题

调用函数时，log 信息，并且可以动态配置输出信息

##### 解决

```

	from functools import wraps, partial
	import logging
	
	# Utility decorator to attach a function as an attribute of obj
	def attach_wrapper(obj, func=None):
		if func is None:
			return partial(attach_wrapper, obj)
		setattr(obj, func.__name__K, func)
		return func

	def logged(level, name=None, message=None):
		def decorate(func):
			logname = name if name else func.__module__
			log = logging.getLogger(logname)
			logmsg = message if message else func.__name__
			
			@wraps(func)
			def wrapper(*args, **kwargs):
				log.log(level, logmsg)
				return func(*args, **kwargs)
			
			@attach_wrapper(wrapper)
			def set_level(newlevel):
				nonlocal level
				level = newlevel
			
			return wrapper
			
		return decorate
	
	@logged(logging.DEBUG)
	def add(x, y):
		return x + y
	
	>>> add.set_level('logging.WARNING')
```

##### 讨论

这一节关键点在「访问函数」（`set_level()`），它被作为属性赋给「包装函数」。每个「访问函数」允许使用 `nonlocal` 来修改函数内部的变量。

### 带可选参数的装饰器

##### 解决

```

	def logged(func=None, *, level=logging.DEBUG):
		if func is None:
			return partial(logged, level=level)
		
		@wraps(func)
		def wrapper(*args, **kwargs):
			log.log(level, logmsg)
			return func(*args, **kwargs)
		
		return wrapper
```

##### 讨论

* 编程一致性：选 `@logged` 还是 `@logged()`
* `functools.partial` 



