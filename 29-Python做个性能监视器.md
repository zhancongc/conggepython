# 绘制性能曲线图

绘图工具：matplotlib

数据来源：psutil；time

方案：

监视一段时间内的CPU使用率，并计算平均值，生成CPU性能曲线

```python
import time
import psutil as ps
import matplotlib.pyplot as plt

PERIOD = 100
times = 0
t = list()
cpu_percent = list()
start_time = time.time()
while times < PERIOD:
    t.append(time.time() - start_time)
    cpu_percent.append(ps.cpu_percent())
    # 清除所有活动坐标轴
    plt.clf()
    average_cpu_percent = sum(cpu_percent) / len(cpu_percent)
    plt.title("CPU performance, average: {0}%".format(round(average_cpu_percent, 2)))
    plt.xlabel("time(second)")
    plt.ylabel("cpu percent(%)")
    plt.ylim(0, 100)
    plt.plot(t, cpu_percent)
    plt.pause(0.1)
    times += 1
print(cpu_percent)
plt.savefig("./cpu_percent.png")
```