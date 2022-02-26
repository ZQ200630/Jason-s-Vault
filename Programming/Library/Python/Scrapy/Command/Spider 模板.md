---
tags:  自动化 Python Scrapy
alias: 
---
# Spider 模板

```python
[[CrawlSpider]]
allow 
#满足括号中“正则表达式”的值会被提取，如果为空，则会全部匹配。
deny 
#与这个正则表达式(或正则表达式列表)不匹配的URL一定不提取
allow_domains 
[[会被提取的链接domains]]。
deny_domains 
[[一定不会被提取链接的domains]]
restrick_xpaths 
[[使用xpath表达式，和allow]]共同作用过滤链接
```