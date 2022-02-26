---
tags:  自动化 Python
alias: 
---
# 系统操作模块 os

```python
mkdirs()  
# 创建文件夹
path()
    abspath(path)  
    # 返回path的绝对路径
    isabs(path)  
    # 判断path是否为绝对路径
    relpath(path, start)  
    # 返回start到path的相对路径
    basename(path)  
    # 返回path最后一个/后面的内容
    dirname(path)  
    # 返回path最后一个/前的内容
    split()  
    # 返回上述两个方法所获得的元组
    getsize(path)  
    # 返回path文件字节数
    exists(path)  
    # 检查是否存在文件或文件夹
    isfile(path)  
    # 价差是否为文件
    isdir(path)  
    # 检查是否为文件夹
listdir(path)  
# 列出所有文件
unlink(path)  
# 删除path处文件
    rmdir(path)  
    # 删除path处文件夹(文件夹必须为空)
walk(path)  
# 遍历目录树,返回值分别为文件夹名称字符串,文件夹中子文件夹的字符列表,当前文件夹中文件的字符串的列
```