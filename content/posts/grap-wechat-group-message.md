---
date: 2019-05-22
tags:
  - 微信
  - python
  - 群消息
title: py 自动抓取微信群消息
slug: grap-wechat-group-message
share: true
canonicalURL: ""
keywords: 
description: 
series:
  - Tools
lastmod: 
lang: cn
Cover:
  image: /images/grap-wechat-group-message-20240116234653481.webp
author: hellloveyy
---


{{< figure src="/images/grap-wechat-group-message-20240116234653481.webp" caption="">}}
## 准备

1. 准备一个 17 年之前的微信号，新账号有限制不可以登录网页微信扫码
    
2. 准备国内的域名和服务器
    

## 过程

1. 准备机器定时运行 py 脚本（可以用别的写） 本人微信自我检测时间验证最长为 1 天 12 个小时，最短几个小时，可以 crontab 设置10分钟检测登录状态，没有登录则发送微信或者邮件（免费 Mailgun）登录微信扫码图片，扫码登录即可
    
2. 检测脚本抓到的固定群里面的固定信息（自己定义群名和信息），自己处理即可，无论入库分析，还是实时发送指定消息,可自由发挥


## 脚本例子

```python
# 抓取所有加入“拼车”群名的群消息，找到包含“车找人”的消息，写入到wechat.log文件中

#!/usr/bin/python

# coding:utf8

import logging,logging.handlers

import itchat

import time

from itchat.content import *

logging.basicConfig()

myapp = logging.getLogger('myapp')

myapp.setLevel(logging.INFO)

filehandler = logging.handlers.TimedRotatingFileHandler("./wechat.log", when='D', interval=1)

filehandler.suffix = "%Y-%m-%d.log"

myapp.addHandler(filehandler)

# 注册文字信息 设置为群聊信息

@itchat.msg_register([TEXT], isGroupChat=True)

def group_reply_text(msg):

if msg['Type'] == TEXT:

content = msg['Content']

if '车找人' in content:

myapp.info('||%s' %content)

itchat.auto_login(enableCmdQR=2)

# 获取所有通讯录中的群聊

# 需要在微信中将需要同步的群聊都保存至通讯录

itchat.get_chatrooms(update=True)

chatrooms = itchat.search_chatrooms(name='拼车')

itchat.run()

```