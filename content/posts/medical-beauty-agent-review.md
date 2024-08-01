---
date: 2024-07-01
tags:
  - 产品
  - 智能客服
  - agent
title: 现阶段做智能客服 agent 的复盘（持续补充）
slug: medical-beauty-agent-review
share: true
canonicalURL: 
keywords: 
description: 
series:
  - agent
  - 产品
  - 复盘
lastmod: 
lang: cn
Cover:
  image: /images/medical-beauty-agent-review-20240730173417791.webp
author: hellloveyy
---



{{< figure src="/images/medical-beauty-agent-review-20240730171108389.webp" caption="">}}

> 有一些动作在规划中没有实际动作，因为后期智能客服项目有交接，不过作为一个真心喜欢 AI 行业的 PM，思考是不能停的，时刻保持空杯的心态

老规矩先总结：效果还是不错的嘿😁
{{< figure src="/images/medical-beauty-agent-review-20240801171614782.webp" caption="">}}
{{< figure src="/images/medical-beauty-agent-review-20240801171655478.webp" caption="">}}
{{< figure src="/images/medical-beauty-agent-review-20240801171924357.webp" caption="">}}

- “屎上雕花“ 是必不可少的，快速达到 80%的预期效果，剩下的 20%就需要沉下心来，深入到业务场景，不要觉得很细碎，好与不好就在这细碎之间，所有做的事情做好归类，最后来看的时候会发现，这也不少啊，而且多数经验在以后的 AI 项目中都可以用到。
- 非常小且快速的意图识别分类模型，用来做什么呢？
	- 前置回复的判断, 例如：正品验证固定话术，销售话术触发
	- function call 的补充调用，例如：调用外部地图 API 查询路线信息
	- 加载指定知识库内容，例如：询问最近的店内优惠，某医生的坐诊信息
- 对话内容变量提取模型和策略，用来做什么呢？
	- 推荐机构内符合提取变量的案例，商品，医生，例如：推荐一个价格在 1w 的注射除皱商品
- 重心放在做知识图谱，做长远的规划，而不是纯 RAG，数据是 LLM 的天花板，高质量的有结构数据是板中板。在去年的时候就跟公司申请要做但是一直没有批下来唉。
- 如果要做好智能客服这种项目，一个专业的金牌销售是你最终要打成的目标，团队里面具有这样的一个人用来分析构建 agent 的 workflow 是一大幸事，然而我并没有哈哈哈，辛亏自己也做过销售。
- 做机构独有数据整合，大部分回复差的 case 都是因为信息的缺失以及不准确
	- 根据对话中的标志比如商品 id, 来聚合所有相关商品的问题，能收拢回复 90%以上的问题
	- 自然私信中产生的问题 QA 依赖提示词+大模型不断提取
	- 电话销售产生的录音记录也可以提取优质对话
	- 机构未上商品到平台，但机构内有的商品 
	- 机构的预约信息处理 
	- 院内具有的仪器信息 
	- 院内定期活动优惠信息
- 光回复问题达不成销售的目的，需要归类优质的引导销售话术，金牌销售都是有套路的，可以基于大的类目，每一个类目做一个提示词上的 few-shot 示例
- 可以尝试做人的 RAG, 大部分的人具有相似的问题，把知识库挂在人的概念上，以对话内容做向量匹配不同人的问题库
- 对召回内容增加一个 workflow 判断信息点是否和聊天内容相关，避免非相关内容影响答案
- 每一个问题，先结合上下文重新思考与拆分之后，重新生成，再去检索。单独模型，用来改写 query，使用历史的相关问题作为数据集  
	- 例如：
	- 用户： 约个时间注射 20U 单位的伊婉C
	- AI 客服：亲，好的您直接下单到院即可，我们会有专人客服接待 
	- 用户：我上次注射的那个现在多少钱？ 
	- 最后一个问题，“我上次注射的那个商品还有么？ ”，是模糊的。
	- 最后一个问题应该被重写为：“上次注射的 20 U 单位的伊婉 C 现在多少钱”

```
方式一直接在当前prompt中增加一个工作流提示词节点：
Query 改写： rephrase an improved and expanded version of user question. Ensure that it provides more nuanced context and details while reducing potential ambiguities  

方式二单独调用对话，单独模型重写query：
给定以下对话，重写最后一个用户输入以反映用户实际在问什么。
【历史对话】
```
