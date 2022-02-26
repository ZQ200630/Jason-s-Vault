---
tags:  自动化 Python
alias: 
---
# 邮件管理模块 smtplib

```python
smtpObj = smtplib.SMTP('smtp.qq.com', 25)  
# 指定smtp服务器
smtpObj.ehlo()  
# 像服务器发起连接请求
smtpObj.starttls()  
# TLS加密
smtpObj.login('2814220624@qq.com', 'cdpfjjsuiydudcfc')  
# 登录
print(smtpObj.sendmail('2814220624@qq.com','m17607103874@163.com', 'Subject: Lont time no see'))  
# 发送邮件
smtpObj.quit()  
# 退出
```