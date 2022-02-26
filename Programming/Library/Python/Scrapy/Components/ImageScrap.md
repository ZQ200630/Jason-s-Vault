---
tags:  自动化 Python Scrapy
alias: 
---
# ImageScrap

## **Item**

```python
 import scrapy
 ​
 class ImageItem(scrapy.Item):
     [[注意这里的item是ImageItem]]   
     image_urls = scrapy.Field()
     images = scrapy.Field()
 ​
 [[image_urls和images]]是固定的,不能改名字
```

## **Settings**

```python
USER_AGENT = 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36'
[[上面只是个访问header]]，加个降低被拒绝的保险
  
ITEM_PIPELINES = {
		'scrapy.pipelines.images.ImagesPipeline': 1
}
[[打开Images]]的通道
  
IMAGES_STORE = 'Path'
[[一定要设置这个存储地址，要是真实的硬盘，可以不创建pics文件夹，会自己生成，还生成默认的full]]文件夹
```