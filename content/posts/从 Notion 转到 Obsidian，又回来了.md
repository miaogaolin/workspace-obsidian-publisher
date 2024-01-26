---
date: 2023-02-10T12:43:25+08:00
tags:
  - app_obsidian
title: 从 Notion 转到 Obsidian，又回来了
slug: back-notion
share: true
canonicalURL: 
keywords:
  - Notion
  - obsidian
description: 
series: 
lastmod: 
cover:
  image: https://images.unsplash.com/photo-1619669906274-d379128eaa59?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw0MHx8Y2lyY2xlfGVufDB8MHx8fDE3MDYyNDQ2MjF8MA&ixlib=rb-4.0.3&q=80&w=7200
author: 
---


> 2024 年我又投入到了 Obsidian 发布文章了，原因自然是找到了更流程的发布方式，详细请看 《[使用 Obsidian 免费建个人博客]({{< relref "%E4%BD%BF%E7%94%A8%20Obsidian%20%E5%85%8D%E8%B4%B9%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}})》
# 为啥回来呢

- 主要是 Notion 模板的发布，我优化了下，可以正常使用了
- 我的网站，我不用依赖双链
- Notion 的模板够好看
- 部署起来很方便
- 写文章足够自由省事
- Notion AI

当然这只是我的选择，Notion 部署也有缺点，但我不太在乎，毕竟每个人的需求点不一样。

如何部署，看这篇： [Vercel + Notion 建个人博客]({{< relref "Vercel%20+%20Notion%20%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}})

---

# 为何使用

我其实一直在使用 Notion，也使用 Notion 建立起了自己的个人网站，[LaoMiao.site](https://laomiao.site/)。

> 所有体验都基于免费基础上，希望在免费基础上有很好的体验

该个人网站基于该项目 [nextjs-notion-starter-kit]([https://github.com/transitive-bullshit/nextjs-notion-starter-kit](https://github.com/transitive-bullshit/nextjs-notion-starter-kit))，使用 Vercel 发布，但在使用中出现了如下问题：

- 该项目访问太慢，尤其大图片的影响
- nextjs-notion-starter-kit 出现问题不能及时解决，当然自己也可以修改，但是难度比较大（是我太笨了）
- Vercel 页面容易出现失败，当 Notion 页面太多，Function 时间如果处理超过 10s（免费额度），就失败了

当然，使用 Notion 作为后台也有他的优点让我选择：

- 无感发布，在 Notion 写好的内容无感进行发布
- 有 Notion 作为后台，编写体验比较好

为了解决上面的问题，并延续优点，我转入了 obisdian 的怀抱，当然也有其它优点：

- 内容都在本地存储，没有风险
- 不同于 Notion 的其它体验
- 基于 Markdown 语法，并基于本地内容生成文章，只是还不能实现无感，但好在内容编辑体验比较好，这个问题不大

现在寻找下基于 obsidian 存在的网站模板有哪些？

# Obsidian账户

使用时不需要进行登录就可以使用，笔记都存储在本地，只需要用 Onedrive 类似的工具同步即可，当然 Git 也 Ok。
那使用账户登录时，是为了得到一些付费的额外体验：

1. 商业授权
2. 笔记可以在网络中公共访问
3. 笔记随账户同步

# 插件

打开第三方插件：Settings -> Community plugins -> Browse。
安装好的插件，在 Settings -> Community plugins 下图位置启用：

![eba9a2acb3d535622e5868c6f023ff33.webp](/images/eba9a2acb3d535622e5868c6f023ff33.webp)

## Picgo

![b2d2f97009d478019621938221a8f8f1.webp](/images/b2d2f97009d478019621938221a8f8f1.webp)

打开插件设置，选择：

![0ca3c85c1f5f536232508afe7efcff2e.webp](/images/0ca3c85c1f5f536232508afe7efcff2e.webp)

前提需要在本地提前安装好该应用，我本地配置的是图床是 Gitee。
下来，直接粘贴图片即可。

# 模板

在写 Hugo 文章时，开始需要先写 metadata 数据，类似：

```
---
title: "111"
---
```

为了解决这种每次的繁琐事情，可以写一个模板，直接使用即可。

![6bd5a2013a1fb978ab8747d8e6953915.webp](/images/6bd5a2013a1fb978ab8747d8e6953915.webp)

## 快捷方式

方法一，命令行：cmd/ctrl + p -> Templates: insert ...

方法二，绑定快捷键：Settings -> Hotkeys -> Insert templates

# 网站发布

## 使用 Hugo 模板

Hugo: [Install Hugo with "extended" Sass/SCSS version](https://gohugo.io/getting-started/installing/)，记着安装的必须带 Sass，不然在运行

```bash
make serve
```

命令时，提醒错误：

![1cd384ffa9fdf5591e292fdf1ef3978f.webp](/images/1cd384ffa9fdf5591e292fdf1ef3978f.webp)

开源项目 [https://github.com/jackyzha0/quart](https://github.com/jackyzha0/quart) 克隆到本地，进入根目录，运行命令：

```bash
make update
make serve
```