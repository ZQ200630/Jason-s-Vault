---
tags:  自动化 Python
alias: 
---
# 二进制持久化对象模块 shelve

```python
with shelve.open('spam') as db:
    db['eggs'] = 'eggs'
# 写    
s = shelve.open('test_shelf.db')
value = s['kk']
# 读
```