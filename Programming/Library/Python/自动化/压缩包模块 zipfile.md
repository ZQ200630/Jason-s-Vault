---
tags:  自动化 Python
alias: 
---
# 压缩包模块 zipfile

```python
ZipFile(path)  
# 读取一个zip文件
    namelist() 
    # 返回zip文件中的文件列表
    extractall()  
    # 解压缩所有文件
    extract(name, path)  
    # 解压缩某文件至某位置
    close()  
    # 关闭文件
    getinfo(name)  
    # 返回某一文件信息
        file_size  
        # 文件大小
        compress_size 
        # 压缩后大小
ZipFile(name, 'w')  
# 以写模式打开zip对象,若追加写入则加一个a
    write('filename', compress_type=zimfile.ZIP_DEFLATED)
```