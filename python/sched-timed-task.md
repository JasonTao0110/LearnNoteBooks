




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

