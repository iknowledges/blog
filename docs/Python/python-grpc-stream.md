# Python使用Grpc中的stream

python中使用双向stream时没有Read和Write方法，于是采用如下替代方案，iter方法可以将一个可迭代对象转成迭代器，next方法从迭代器中获取下一个元素，没有更多元素时触发StopIteration异常。

```python
import queue

send_queue = queue.SimpleQueue()  # or Queue if using Python before 3.7
my_event_stream = stub.MyEventStream(iter(send_queue.get, None))

# send
send_queue.put(StreamingMessage())

# receive
response = next(my_event_stream)  # type: StreamingMessage
```

#### 参考资料

- [How do i handle streaming messages with Python gRPC](https://stackoverflow.com/questions/47831895/how-do-i-handle-streaming-messages-with-python-grpc)
