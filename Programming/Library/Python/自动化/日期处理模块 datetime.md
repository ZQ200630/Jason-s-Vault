---
tags:  自动化 Python
alias: 
---
# 日期处理模块 datetime

```python
datetime.now()  
# 返回datetime对象，包含当前日期和时间
datetime(2015, 10, 21, 16, 29, 0)  
# 传入年月日时分秒构造一个时间对象,分别保存在year, month, day, hour, minuite, second中
datetime.datetime.fromtimestamp(seconds)  
# 返回Unix纪元后的seconds秒的时间对象
         strftime('%Y/%m/%d $H:%M:%S')  
    # 格式化datetime返回字符串
timedelta(days=, hours=, minutes=, seconds= )  
# 创建一个储存时间的对象，其可以与上述的datetime相加返回一个新的datetime
datetime.strptime('string'， format)  
# 通过format从string中初始化datetime
Palse to accurate date：
    date = datetime.datetime(.....)
    while datetime.datetime.now() < date:
        time.sleep(1)
```