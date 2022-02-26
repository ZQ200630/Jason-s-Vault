---
tags:  自动化 Python
alias: 
---
# PDF处理模块 PyPDF2

```python
pdfFileObj = open('path', 'rb')  
# 通过二进制格式打开一个pdf文档
reader = PyPDF2.PdfFileReader(pdfFileObj)  
# 读入该pdf对象
reader.getPageNumber()  
# 返回该pdf对象页数
page = reader.getPage(50)  
# 返回第五十页pdf
page.extractText()  
# 返回提取出来的文字字符串
page.rotateClock(angle)  
# 将page旋转一个角度
page.mergePage(page1)  
# 将page1叠到page上
reader.isEncrypted  
# 是否加密了(若加密无法读取文件)
reader.decrypt(password)  
# 通过password解锁文件,返回值是否解锁   
Copy pages：
    writer = PyPDF2.PdfFileWriter()
    writer.addPage(page)  
    # 在writer尾部添加一个页面
    writer.write(file)  
    # 把更改的writer写入某个pdf文件
    writer.encrypt(password)  
    # 加密pdf文档
```