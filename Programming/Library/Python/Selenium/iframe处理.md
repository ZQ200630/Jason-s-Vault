---
tags:  自动化 Python Selenium
alias: 
---
# iframe处理

```python
driver.switch_to.frame('id or name') [[进入iframe]]
driver.find_element()
driver.switch_to.default_content() [[退出iframe]]

# iframe嵌套则先定位外层iframe再定位内层iframe
# 无论进入几层iframe，只要退出就是最外层
[[如果iframe没有明确的id或name，可以利用xpath]] driver.find_element_by_xpath定位
```