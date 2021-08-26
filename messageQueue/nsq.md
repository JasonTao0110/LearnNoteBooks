### 消息中间件


```python
# mq.py
class Reader:
    def __init__(self, options):
        self.options = options

    def run(self, callback):
        pass

class Writer:
    def write(self, message, *, callback=None):
        pass
```

```python
import nsq
import requests
from .mq import Writer, Reader

# NSQ 的api支持的不好，暂时使用http接口，性能不好
class NsqWriter(Writer):
    def __init__(self, *, addrs, topic):
        
        self.url = "http://{}/pub?topic={}".format(
            addrs[0], 
            topic
        )

    def write(self, message, *, callback=None):
        requests.post(self.url , data=message)

class NsqReader(Reader):
    def run(self, callback):
        def _mission(message):
            if message.body:
                callback(message.body, message=message)

            message.finish()

        nsq.Reader(
            message_handler=_mission,
            **self.options
        )

        nsq.run()         
```


```python
# 使用
class App():
    def __init__(self):
        ...
    
    ...

    def get_nsq_writer(self):
        if self.nsq == None:
            addrs = app_info.get_config("nsq_asr", "addr")
            topic = app_info.get_config("nsq_asr", "write_topic")
            self.nsq = NsqWriter(addrs=addrs, topic=topic)

        return self.nsq

app_info = App()

...

app_info.get_nsq_writer().write(message_task.dumps())

```