
## sched

```python
# scrapy 定时任务
from scrapy import cmdline
import time
import sched
import sys
import os

# 周期性执行给定的任务
# 初始化 sched 模块的scheduler类
# 第一个参数是一个可以返回时间戳的函数，第二个参数可以在定时未到达之前阻塞。
schedule = sched.scheduler(time.time, time.sleep)


# 被周期性调度触发的函数
def start_scrapy():
    os.system("scrapy crawl qq --nolog")


def perform(inc):
    schedule.enter(inc, 0, perform, (inc,))
    start_scrapy()  # 需要周期执行的函数


# 每30 * 60 s执行一次(30分钟)
def main_scrapy(inc=1800):
    """
    schedule.enter(delay, priority, action, argument=())
    用来加入调度事件，即将任务加入到队列中，它有四个参数，分别为：
    间隔时间、
    优先级（为两个被调度在相同时间执行的函数定序，数字越小，优先级越高）、
    被调用触发的函数、
    函数的参数（参数放在元组中，当只有一个参数时，写为(parm,)）
    """
    schedule.enter(0, 0, perform, (inc,))
    schedule.run()  # 开始执行队列中的任务，直到计划时间队列变成空为止


if __name__ == "__main__":
    main_scrapy()

```


## schedule
```python

import json
from sys import stdout

import schedule
import time
import datetime
import requests
from kafka import KafkaProducer

import applib

applib.load_svc_env()


class Producer:
    def __init__(self):
        self.broker_list = []
        self.topic = ""
        self.producer = KafkaProducer(bootstrap_servers=self.broker_list)

    def sent_success(self, *args, **kwargs):
        print("sent_success：", args)
        return args

    def sent_error(self, *args, **kwargs):
        print("sent_error：", args)
        return args

    def produce(self, value):
        print(self.topic, self.broker_list)
        # print(json.dumps(value).encode("utf-8"))
        self.producer.send(topic=self.topic, value=json.dumps(value).encode("utf-8")). \
            add_callback(self.sent_success).add_errback(self.sent_error)
        time.sleep(2)

        stdout.flush()


def scrapy_task():
    p = Producer()
    print("I'm working scrapy ...", datetime.datetime.now())
    p.produce(value={"site": "qq"})
    time.sleep(2)
    p.produce(value={"site": "iqiyi"})
    time.sleep(2)
    p.produce(value={"site": "mgtv"})



# 每10分钟执行一次任务            
schedule.every(10).minutes.do(scrapy_task)
# 每小时执行一次任务              
schedule.every().hour.do(scrapy_task)
# 每天在什么时间点执行一次任务    
schedule.every().day.at('10:30').do(scrapy_task)
# 每5-10分钟(随机)执行一次任务    
schedule.every(5).to(10).minutes.do(scrapy_task)
# 每周一执行一次任务              
schedule.every().monday.do(scrapy_task)
# 每周一什么时间点执行一次任务    
schedule.every().monday.at('9:30').do(scrapy_task)
# 每分钟在第17秒的时候执行任务    
schedule.every().minute.at(':17').do(scrapy_task)

while True:
    schedule.run_pending()
    time.sleep(1)

```