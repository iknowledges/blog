# asyncio异步编程指南

## 事件循环

调用协程函数创建协程对象，函数内部代码不会立即执行，必须将协程对象放入事件循环中才会开始执行。

```python
import asyncio

# 协程函数
async def func():
    print("call func")

# 返回协程对象
result = func()

# 开启事件循环
# python3.7之前
# loop = asyncio.get_event_loop()
# loop.run_until_complete(result)
# python3.7之后
asyncio.run(result)
```

## await

await + 可等待的对象（协程对象、Task、Future）

```python
import asyncio

async def func():
    print("start")
    await asyncio.sleep(2)
    print("end")
    return "func result"

async def main():
    # 遇到IO操作挂起当前协程，事件循环可以去执行其他协程
    response1 = await func()
    print("response1:", response1)

    # 这里response2会等待response1执行完才开始执行
    response2 = await func()
    print("response2:", response2)

asyncio.run(main())
```

## Task对象

Task用于向事件循环中添加多个任务，通过create_task(协程对象)Task对象，这样可以让协程加入事件循环中等待被调度执行。

- 示例1

```python
import asyncio

async def func():
    print("start")
    await asyncio.sleep(2)
    print("end")
    return "func result"

async def main():
    # 创建Task对象
    task1 = asyncio.create_task(func())
    task2 = asyncio.create_task(func())

    print('main finished')

    # 当执行某协程遇到IO操作时，会自动切换执行其他任务
    ret1 = await task1
    print("task1:", ret1)
    # 这里task2不会等待task1完成才执行
    ret2 = await task2
    print("task2:", ret2)

asyncio.run(main())
```

- 示例2：

```python
import asyncio

async def func():
    print("start")
    await asyncio.sleep(2)
    print("end")
    return "func result"

async def main():
    task_list = [
        asyncio.create_task(func(), name='t1'),
        asyncio.create_task(func(), name='t2')
    ]

    print('main finished')

    # 如果等待超时任务没执行完，pending就返回被挂起的任务
    done, pending = await asyncio.wait(task_list, timeout=5)
    print('done:', done)
    print('pending:', pending)

asyncio.run(main())
```

## Future对象

Task继承了Future，Task对象内部await结果的处理时基于Future对象的。

```python
import asyncio

async def work(fut):
    await asyncio.sleep(2)
    fut.set_result("666")

async def main():
    # 获取当前事件循环
    loop = asyncio.get_running_loop()

    # 创建Future对象
    fut = loop.create_future()

    # 创建Task对象
    await loop.create_task(work(fut))

    res = await fut
    print(res)

asyncio.run(main())
```

## concurrent.futures.Future

```python
import time
from concurrent.futures.thread import ThreadPoolExecutor
from concurrent.futures.process import ProcessPoolExecutor

def work(value):
    time.sleep(1)
    print(value)
    return 'work done'

# 创建线程池
pool = ThreadPoolExecutor(max_workers=5)

# 创建进程池
# pool = ProcessPoolExecutor(max_workers=5)

for i in range(10):
    fut = pool.submit(work, i)
    print(fut.result())
```

- 混合使用示例：

```python
import time
import asyncio
import concurrent.futures

def work():
    time.sleep(2)
    return 'work done'

async def main():
    loop = asyncio.get_running_loop()

    # 使用默认线程池ThreadPoolExecutor
    fut = loop.run_in_executor(None, work)
    result = await fut
    print('default thread pool:', result)

    # 使用自定义的线程池
    # with concurrent.futures.ThreadPoolExecutor() as pool:
    #     fut = loop.run_in_executor(pool, work)
    #     result = await fut
    #     print('custom thread pool:', result)

    # 使用自定义的进程池
    # with concurrent.futures.ProcessPoolExecutor() as pool:
    #     fut = loop.run_in_executor(pool, work)
    #     result = await fut
    #     print('custom process pool:', result)

asyncio.run(main())
```

## 异步迭代器

异步迭代器是实现了__aiter__()和__anext__()方法的对象。__anext__必须返回一个awaitable对象。async for会处理异步迭代器的__anext__()方法所返回的可等到对象，直到触发一个StopAsyncIteration异常。

```python
import asyncio

# 自定义异步迭代器
class Reader(object):
    def __init__(self):
        self.count = 0
    
    async def readline(self):
        await asyncio.sleep(1)
        self.count += 1
        if self.count == 10:
            return None
        return self.count
    
    def __aiter__(self):
        return self
    
    async def __anext__(self):
        val = await self.readline()
        if val == None:
            raise StopAsyncIteration
        return val


async def main():
    obj = Reader()
    async for item in obj:
        print(item)

asyncio.run(main())
```

## 异步上下文管理器

异步上下文管理器通过定义__aenter__()和__aexit__()方法来对async with语句中的环境进行控制。

```python
import asyncio


class AsyncContextManager:
    def __init__(self):
        self.conn = None

    async def do_something(self):
        # 异步操作数据库
        return 666
    
    async def __aenter__(self):
        # 异步连接数据库
        self.conn = await asyncio.sleep(2)
        return self
    
    async def __aexit__(self, exc_type, exc, tb):
        # 异步关闭数据库连接
        await asyncio.sleep(2)

async def main():
    async with AsyncContextManager() as f:
        result = await f.do_something()
        print(result)

asyncio.run(main())
```

## uvloop

- 安装

```
pip3 install uvloop
```

- 使用uvloop来加速asyncio

```python
import asyncio
import uvloop

asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())

# 编写asyncio代码，和之前一样
```

#### 参考资料

- [asyncio异步编程](https://www.bilibili.com/video/BV1qZaszaERH)