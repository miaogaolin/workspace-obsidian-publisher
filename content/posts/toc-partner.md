---
date: 2024-03-08
tags:
  - 大模型
  - 产品
  - 伴侣
  - agent
  - ToC
title: ToC端医美AI伴侣产品-简述（一期）
slug: toc-partner
share: true
canonicalURL: ""
keywords: 
description: ""
series:
  - 产品
  - agent
lastmod: 
lang: cn
Cover:
  image: /images/toc-partner-20240308145423357.webp
author: hellloveyy
---

> 持续更新中... 文字后续补充

## 一、产品框架脑图

{{< figure src="/images/toc-partner-20240308142653848.webp" caption="">}}

**区别于目前 ToB 智能客服：**

1. 意图识别需要更为精细，涵盖范围更广
2. 增加调用工具模块，包括但不限于多模态介入、输出
3. 记忆能力
4. 实时搜素

## 二、功能架构图

{{< figure src="/images/toc-partner-20240308142708466.webp" caption="">}}


## 三、业务流程图 SOP

{{< figure src="/images/toc-partner-20240308142716841.webp" caption="">}}

## 四、功能详述

### 一、企微部分

1. 公众号：
    
    - 增加菜单 私人医美伴侣
        
    - 角色选择（内置）: 点击进入落地页，展示不同人设的企微个人微信二维码
        
    - 提供不同的捏人选择
        
    
2. 添加好友
    
    - 触发介绍语
        
    - 存储用户唯一标识
        
3. 企业微信接口对接 
    
    - 账号：企微公司账号
        
    - 服务商后台：https://open.work.weixin.qq.com/wwopen/developer#/sass/apps/list
        
    - 注意！需要实现企微个人号，目前咨询企微内部人员并未开通企微个人号收发消息接（一期如果企微个人号暂时无法实现，可用微信个人号暂时代替，目标为打通整体流程）
        
        - 自研调研：
	        - https://github.com/cheungchazz/WeChat-AIChatbot-WinOnly?tab=readme-ov-file
                
            - https://wechaty.readthedocs.io/zh-cn/latest/
                
            - https://github.com/wechaty/wechaty#readme
                
            - https://github.com/wechaty/puppet-padlocal
                
                - 官方discard ：https://discord.gg/72VjQGzm
                    
            - https://github.com/leochen-g/wechat-assistant-pro?tab=readme-ov-file
                
        - 第三方采买：
            
            - 集简云[AI智能销售/客户服务/内部咨询解决方案](https://w8nftt58s2.feishu.cn/docx/EodadWNGioprMFxcXwQccpiqnAc)
                
            - LinkAI

### 二、微信交互

1. 用户输入发送内容
    
    - 文字：支持
        
	- 链接url：支持
            
    - 语音：支持
        
    - 图片：支持
        
        - 前置回复：“嗯嗯 好漂亮啊，我能测脸型、发型、眼型、颜值、皮肤、眉形，还能模拟整形和 美妆模拟，所以你想测测啥😁”
            
        - 用户回复内容判断意图识别调用不同的图像能力 tools
            
    - 文件（Word、Pdf、Excel）：一期不支持
        
    - 视频：一期不支持
        
    - 其他：一期不支持
        
2. 输出内容
    
    - 拆分回复
        
    - 多模态回复
        
        - 语音回复调研 https://github.com/RVC-Boss/GPT-SoVITS/blob/main/docs/cn/README.md
            
    - 粘性追问
        
3. 延迟等待 3s 统一进行回复
    
4. 每日主动推送，以下内容随机推送一条
    
    - 功能推荐消息
        
        - 拟人话术举例： "嘿，跟你说个事儿 你发我一个正面照片儿 我能帮你检测检测皮肤奥" 。基于以上的内容，需保证询问的随机性
            
    - 了解用户消息
        
        - 拟人话术举例： "哦 对了 你在哪个城市啊 天气咋样" 。基于以上的内容，需保证询问的随机性
            
    - 医美小知识 图文推送
        
        - 拟人话术举例： "哈喽 今天给你科普一下{知识点}" 。基于以上的内容，需保证询问的随机性
            
        - 结合用户画像（医美相关信息）+ 百科内容随机推送
            
5. 聊天数据存储

### 三、意图识别

{{< figure src="/images/toc-partner-20240308142732164.webp" caption="">}}

### 四、工具模块

### 五、记忆模块

### 六、Prompt 构建

