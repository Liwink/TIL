## rq

1. Worker 同时只执行一个 job，所以要并行需要启动多个 worker。

2. Worker 会 fork 出一个子进程执行 job。

   > *Fork a child process.* A child process (the "work horse") is forked off to do the actual work in a **fail-safe context**.

3. 作者将 daemon 包装为 class Worker，改进包括 fork 子进程执行、可访问执行状态。

   > Add an actual awesome worker structure.

4. 用 pickle 将原始函数和参数打包，放入 redis 队列中。

5. DelayResult 封装结果返回函数，异步从 redis 读取 result。

6. commit 6th: class Queue()。封装了 worker 和 redis 的连接，暂时只是存储 to_queue_key

   > Factor out a Queue object.
   >
   > It might be useful to add some methods to that object, later.

7. commit 10th: 用 stack 管理 redis connection

   > Add better connection management.
   >
   > To start using RQ, push a Redis connection up its stack, like so:
   >
   > ```python
   > from rq import push_connection
   > push_connection(Redis())
   > ```

   这里引用了 `werkzeug` 中的 [LocalStack](https://github.com/pallets/werkzeug/blob/master/werkzeug/local.py#L89)，本质上是实现了一个 stack。但他管理 connection 优雅的地方并不清楚。

8. Queue, Job, Worker: 后期对 redis 的连接应该会放到 Queue 中，Worker 只需要对 Queue pop push，Queue 中会提供入队的操作，Job 是对连接、函数和处理结果的封装。
   commit 33，移除了 Job，Queue 中完成入队操作，返回结果用 DelayedResult 封装就足够
   commit 40，增加 Job

   >  A Job is just a convenient datastructure to pass around job (meta) data.

9. ConnectionProxy

   ```python
   Find file:
   import redis

   class NoRedisConnectionException(Exception):
       pass


   class RedisConnectionProxy(object):
       def __init__(self):
           self.stack = []

       def _get_current_object(self):
           try:
               return self.stack[-1]
           except IndexError:
               msg = 'No Redis connection configured.'
               raise NoRedisConnectionException(msg)

       def pop(self):
           return self.stack.pop()

       def push(self, db):
           self.stack.append(db)

       def __getattr__(self, name):
           return getattr(self._get_current_object(), name)


   conn = RedisConnectionProxy()

   __all__ = ['conn']
   ```

   几处很有意思

   - `__all__` 限制访问
   - 返回对象实例，而非类
   - 用 stack 保存连接，而非单一

10. commit 39:

    > Restructure some code.
    >
    > No functional change, but leave the BLPOP'ing to the Queue, as the queues know how to pop themselves.