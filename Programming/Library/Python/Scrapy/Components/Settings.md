---
tags:  自动化 Python Scrapy
alias: 
---
# Settings

```python
BOT_NAME = 'tencent'

SPIDER_MODULES = ['tencent.spiders']
NEWSPIDER_MODULE = 'tencent.spiders'

# Crawl responsibly by identifying yourself (and your website) on the user-agent
[[USER_AGENT]] = 'tencent (+http:[[www]].yourdomain.com)'  [[更改request]]头, 避免403 Forbidden

# Obey robots.txt rules
ROBOTSTXT_OBEY = True  [[是否遵循robots]]规定,当爬取禁止网站时关闭

# Configure maximum concurrent requests performed by Scrapy (default: 16)
[[CONCURRENT_REQUESTS]] = 32  [[Scrapy]] 一次最大请求数  

# Configure a delay for requests for the same website (default: 0)
# See https:[[doc]].scrapy.org/en/latest/topics/settings.html#download-delay
# See also autothrottle settings and docs
# DOWNLOAD_DELAY = 3  #下载延时
# The download delay setting will honor only one of:
# CONCURRENT_REQUESTS_PER_DOMAIN = 16
# CONCURRENT_REQUESTS_PER_IP = 16

# Disable cookies (enabled by default)
# COOKIES_ENABLED = False  [[设置cookie]]

# Disable Telnet Console (enabled by default)
# TELNETCONSOLE_ENABLED = False

# Override the default request headers:
# DEFAULT_REQUEST_HEADERS = {
#   'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
#   'Accept-Language': 'en',
# }  [[重写默认request]]头部

# Enable or disable spider middlewares
# See https:[[doc]].scrapy.org/en/latest/topics/spider-middleware.html
# SPIDER_MIDDLEWARES = {
#    'tencent.middlewares.TencentSpiderMiddleware': 543,
# }  [[spider]]中间件

# Enable or disable downloader middlewares
# See https:[[doc]].scrapy.org/en/latest/topics/downloader-middleware.html
# DOWNLOADER_MIDDLEWARES = {
#    'tencent.middlewares.TencentDownloaderMiddleware': 543,
# }  #下载器中间件

# Enable or disable extensions
# See https:[[doc]].scrapy.org/en/latest/topics/extensions.html
# EXTENSIONS = {
#    'scrapy.extensions.telnet.TelnetConsole': None,
#}  

# Configure item pipelines
# See https:[[doc]].scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   'tencent.pipelines.TencentPipeline': 300
}  [[pipelines]]打开

# Enable and configure the AutoThrottle extension (disabled by default)
# See https:[[doc]].scrapy.org/en/latest/topics/autothrottle.html
[[AUTOTHROTTLE_ENABLED]] = True
# The initial download delay
[[AUTOTHROTTLE_START_DELAY]] = 5
# The maximum download delay to be set in case of high latencies
[[AUTOTHROTTLE_MAX_DELAY]] = 60
# The average number of requests Scrapy should be sending in parallel to
# each remote server
[[AUTOTHROTTLE_TARGET_CONCURRENCY]] = 1.0
# Enable showing throttling stats for every response received:
[[AUTOTHROTTLE_DEBUG]] = False

# Enable and configure HTTP caching (disabled by default)
# See https:[[doc]].scrapy.org/en/latest/topics/downloader-middleware.html#httpcache-middleware-settings
[[HTTPCACHE_ENABLED]] = True
[[HTTPCACHE_EXPIRATION_SECS]] = 0
[[HTTPCACHE_DIR]] = 'httpcache'
[[HTTPCACHE_IGNORE_HTTP_CODES]] = []
[[HTTPCACHE_STORAGE]] = 'scrapy.extensions.httpcache.FilesystemCacheStorage'
```