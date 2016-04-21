## Tornado IOLoop

##### 继承关系

* `class Configurable(object)` # factory function, global decision

	```
	def __new__(cls):
		impl = cls.configured_class()
	```
* `class IOLoop(Configurable)` # 用户唯一需要接触的类

	```
	def instance()
	def configurable_defualt(cls)
	```
* `class PollIOLoop(IOLoop)`

	```
	def initialize(self, impl, time_func=None, **kwargs)
	def add_handler(self, fd, handler, events) # 程序内部调用的接口
	def start(self) # 启动接口
	```
* `class KQueueIOLoop(PollIOLoop)`

	```
	super(KQueueIOLoop, self).initialize(impl=_KQueue(), **kwargs)
	```