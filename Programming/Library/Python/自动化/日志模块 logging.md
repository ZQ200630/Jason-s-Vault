---
tags:  自动化 Python
alias: 
---
# 日志模块 logging

```python
logging.basicConfig(level=logging.DEBUG, format=' %(asctime)s - %(levelname)s -%(message)s')  
[[创建一个logrecord对象，保存关于该事件细节，level]]表示最低产生日志的等级
debug(message)  
[[按照上述格式打印debug]]
disable(logging.级别)  
#不打印某一级别的信息
```

| Level | Function | Describe |
| --- | --- | --- |
| DEBUG | logging.debug() | 诊断问题时关心的小细节 |
| INFO | logging.info() | 记录程序中的一般时间信息，确保一切正常 |
| WARNING | logging.warning() | 表示可能的问题，不会阻止程序工作，但将来可能会 |
| ERROR | logging.error() | 记录错误，导致程序做某事失败 |
| CRITICAL | logging.critical() | 表示致命错误 |