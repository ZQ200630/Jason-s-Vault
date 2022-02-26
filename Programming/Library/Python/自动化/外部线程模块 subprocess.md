---
tags:  自动化 Python
alias: 
---
# 外部线程模块 subprocess

```python
Popen('programName')  #通过程序名开启外部程序
    poll()  [[如果外部程序执行完毕，则返回0]],否则返回None
    wait()  #停止程序执行，知道外部程序执行完毕
Popen([])  #传递命令行参数，该列表第一个参数为程序名，后面全部都是命令行参数
Popen([]， shell=True)  #利用默认方式打开
```

### 外部任务调度模块 Task Scheduler / Launchd / Cron