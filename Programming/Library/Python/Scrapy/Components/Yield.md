---
tags:  自动化 Python Scrapy
alias: 
---
# Yield

```python
yield scrapy.Response(url=, callback=)  
#继续爬取网页数据
yield items  
[[把item送给pipelines]]处理
```