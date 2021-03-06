# 绘制性能曲线图

准备

绘图工具：matplotlib

数据来源：psutil

安装matplotlib和psutil

```shell
$ pip install -i https://pipy.douban.com/simple matplotlib psutil
```

方案：

记录一段时间内的CPU使用率，并计算平均值，绘制CPU性能曲线。

固定次数

```python
import psutil as ps
import matplotlib.pyplot as plt

TIMES = 100
times = 1
times_list = list()
cpu_percent = list()

while times <= TIMES:
    times_list.append(times)
    cpu_percent.append(ps.cpu_percent())
    # 清除所有活动坐标轴
    plt.clf()
    average_cpu_percent = sum(cpu_percent) / len(cpu_percent)
    plt.title("CPU performance, average: {0}%".format(round(average_cpu_percent, 2)))
    plt.xlabel("times")
    plt.ylabel("cpu percent(%)")
    plt.ylim(0, 100)
    plt.plot(times_list, cpu_percent)
    plt.pause(0.1)
    times += 1
plt.savefig("./cpu_percent.png")
```

固定时间

```python
import psutil as ps
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

cpu_percent = list()
interval = timedelta(seconds=30)
current_time = datetime.now()
future_time = current_time + interval

while datetime.now() < future_time:
    cpu_percent.append(ps.cpu_percent())
    # 清除所有活动坐标轴
    plt.clf()
    average_cpu_percent = sum(cpu_percent) / len(cpu_percent)
    plt.title("CPU performance, average: {0}%".format(round(average_cpu_percent, 2)))
    plt.xlabel("times")
    plt.ylabel("cpu percent(%)")
    plt.ylim(0, 100)
    plt.plot(cpu_percent)
    plt.pause(0.1)
plt.savefig("./cpu_percent.png")
```

方案：

记录一段时间内的内存使用率，并计算平均值，绘制内存占用曲线。

```python
memory_list = list()
interval = timedelta(seconds=30)
total_memory = round(ps.virtual_memory().total/1024**3)
current_time = datetime.now()
future_time = current_time + interval
while datetime.now() < future_time:
    memory_list.append(ps.virtual_memory().percent)
    plt.clf()
    average_memory_percent = sum(memory_list)/len(memory_list)
    plt.title("Memory performance, average: {0}%, total: {1}GB.".format(round(average_memory_percent), total_memory))
    plt.xlabel("times")
    plt.ylabel("cpu percent(%)")
    plt.ylim(0, 100)
    plt.plot(memory_list)
    plt.pause(0.1)
plt.savefig("./memory_percent.png")
```