---
tags:  自动化 Python
alias: 
---
# Word处理模块 docx

```python
doc = docx.Documen('name')  
# 新建一个文档对象
len(doc.paragraphs)  
# 返回段落数
doc.paragraphs[0].text  
# 返回documentTitle，其后调用返回文本字符串
    runs[number].text  
    # 返回每一个runs的text
```