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

### Ⅰ. 企微部分

1. 公众号：
    
    - 增加菜单 私人医美伴侣
        
    - 角色选择（内置）: 点击进入落地页，展示不同人设的企微个人微信二维码
        
    - 提供不同的捏人选择
        
    
2. 添加好友
    
    - 触发介绍语
        
    - 存储用户唯一标识
        
3. 企业微信接口对接 
    
    - 账号：需企微公司账号
        
    - 服务商后台：https://open.work.weixin.qq.com/wwopen/developer#/sass/apps/list
        
    - 注意！需要实现企微个人号，目前咨询企微内部人员并未开通企微个人号收发消息接（一期如果企微个人号暂时无法实现，可用微信个人号暂时代替，目标为打通整体流程，但是个人号封号风险较高）
        
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

### Ⅱ. 微信交互

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

### Ⅲ. 意图识别

1. 建立 AI Intention 意图集模块，方便所有后续 AI 方向 Agent 调用
    
2. 实现途径
    
    - 全输出到Prompt中LLM自行判断，依赖提示词工程
        
    - 前置匹配
        
        - 正则匹配
            
        - 请求 minimax 或本地模型判断当前对话意图
            
3. 入参：私信 => 出参：最终意图
    
    - 单用户私信意图+长对话用户私信意图：最近一条（权重占比高） + 上下文5条判断意图 （权重占比低）
        
        - 语音：输入内容为对应的已转化文本
            
        - 图像：输入内容为 "用户图片文件"
            
    2. 无法判定意图，增加二次询问
        
        - "?????"
            
        - "emm 没太看懂，能展开说说不，或者如果不想打字就发语音呗"
            
        - 基于以上的二次询问内容，保证二次询问的随机性
            
4. 意图判断调用工具逻辑 map
    
    - 历史总结意图分类脑图 
	    {{< figure src="/images/toc-partner-20240308142732164.webp" caption="">}}
        
    - 大模型 + Prompt 兜底非map意图、无法判定意图、其他意图
        
        - 自动收集未命中意图case落表
            
    - 负面情绪："唉不好意思，是我太笨了，要不咱试试 {功能点} "（基于固定回答保持随机性）+ 自动录入负面case数据表
        
    - 图像魔镜： 对应图像检测过后，发出简单概括文本 + 魔镜检测链接落地页
        
        - 测脸型
            
        - 测发型
            
        - 模拟整形
            
        - 测眼型
            
        - 测颜值
            
        - 测皮肤
            
        - 测眉形
            
        - 美妆模拟
            
    - 链接总结
        
    - 定时提醒
        
    - 实时查询 调研实现 https://github.com/leptonai/search_with_lepton
        
        - 天气查询
            
        - 限号查询
            
        - 星座运势
            
        - 时事查询总结
            
    - 个性长期美容计划（获取线索直接对接高定美学转诊，由专业医师+大模型辅助生成时间计划书并且定时提醒用户）


### Ⅳ. 工具模块

1. 建立 AI Tools 工具集，方便所有后续 AI 方向 Agent 调用
    
2. 完成以上意图识别对应工具模块调用
    
3. 额外基础工具
    
    - RAG 百科知识库加载

### Ⅴ. 记忆模块

1. 画像数据存储
    
2. 用户画像字段
    
    - 昵称
        
    - 手机号
        
    - 微信号
        
    - 定位城市
        
    - 性别
        
    - 体重
        
    - 身高
        
    - 当前生活状态：孕期，热恋期，单身，情绪状态
        
    - 医美相关信息
        
        - 医美史
            
        - 做医美的原因
            
        - 想做的项目
            
        - 能接受的治疗手段
            
        - 对疼痛的忍受度
            
        - 审美风格
            
        - 预算范围
            
        - 医生机构选择原则：位置，规模
            
3. 记忆写入画像
    
    - 模型分析聊天记录写入画像 例如：用户发送有一天做了一次热玛吉，需分析写入到医美史中。

### Ⅵ. Prompt 构建

1. 动态结构模块
    
    - 角色人设
        
    - 用户信息
        
    - 聊天记录
        
    - 意图识别 动态加载不同方向 Prompt
        
2. 产品负责最终输出

