---
date: 2024-02-21
tags:
  - Sora
  - OpenAI
title: 有关 Sora 的一些思考和理解
slug: Sora
share: true
canonicalURL: 
keywords: 
description: ""
series:
  - Sora
  - LLM
lastmod: 
lang: cn
Cover:
  image: /images/Sora-20240221151126452.webp
author: hellloveyy
---


{{< figure src="/images/Sora-20240221151018148.webp" caption="">}}

> 持续记录一些自 Sora 发布以来的一些思考
> 官方网址：[OpenAI Sora](https://openai.com/sora)


---

## 2024-02-21
{{< figure src="/images/Sora-20240307162331911.webp" caption="">}}

### **1.视频压缩网络**：

- 功能：将输入的视频或图片压缩成低维度的表示形式，类似于将不同尺寸和分辨率的图片“标准化”，以便于处理和存储。
- 目的：通过降维处理，Sora 能够更高效地处理视觉数据，同时保留足够的信息来重建原始视频内容。

###  **2.空间时间补丁提取**：


{{< figure src="/images/Sora-20240221152133306.webp" caption="">}}

{{< figure src="/images/Sora-20240221152518544.webp" caption="">}}

{{< figure src="/images/Sora-20240221152525196.webp" caption="">}}

{{< figure src="/images/Sora-20240221152538216.webp" caption="">}}

- 功能：将压缩后的视频数据进一步分解为“空间时间补丁”，这些补丁包含了视频内容的基本构建块，即视频中的小块区域及其随时间的变化。

- 目的：通过这种方式，Sora 能够细致地处理视频内容的每一个小片段，并考虑它们随时间的变化，从而更好地理解和生成动态视频内容。

### 3 . **视频生成的 Transformer 模型**：
{{< figure src="/images/Sora-20240221152210156.webp" caption="">}}
- 功能：接收空间时间补丁和文本提示，决定如何将这些片段转换或组合以生成最终的视频。
- 目的：Transformer 模型根据文本提示中的故事，将空间时间补丁组合成连贯的视频内容，讲述文本提示中描述的场景。这个过程通过数百个渐进的步骤完成，每一步都让视频内容更接近最终目标。

### 4 . 目前的局限性
- 物理世界模拟的准确性、长视频生成的困难、准确理解复杂文本指令以及训练与生成效率的挑战
- 优化方式：扩大训练数据集、集成物理引擎、改进训练算法、优化模型结构和利用硬件加速
