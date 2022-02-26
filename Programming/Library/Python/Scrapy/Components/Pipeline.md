---
tags:  自动化 Python Scrapy
alias: 
---
# Pipeline

```python
[[处理图片继承ImagesPipeline]]
    yield scrapy.Response(url)
[[一般处理json]]流程:
    def __init__(self):
        self.f = open(fileName, 'wb')
        self.f.write(b'[')

    def process_item(self, item, spider):
        text = json.dumps(dict(item), ensure_ascii=False)
        self.f.write(text.encode('utf-8'))
        self.f.write(b',')
        self.f.write(b'\n')
        return item

    def close_spider(self, spider):
        self.f.seek(-2, os.SEEK_END)
        self.f.write(b']')
        self.f.close()
```