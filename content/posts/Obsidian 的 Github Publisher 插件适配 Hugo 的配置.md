---
date: 2024-01-14T18:14:22+08:00
tags: 
title: Obsidian 的 Github Publisher 插件适配 Hugo 的配置
slug: github-publisher-hugo
share: true
canonicalURL: 
keywords: 
description: 
series: 
lastmod: 
cover:
  image: https://images.unsplash.com/photo-1653402438643-b230db019d27?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw0NHx8aHVnb3xlbnwwfDB8fHwxNzA1MjI3MjkzfDA&ixlib=rb-4.0.3&q=80&w=400
author: 
---

在 [使用 Obsidian 免费建个人博客{{< relref "](%E4%BD%BF%E7%94%A8%20Obsidian%20%E5%85%8D%E8%B4%B9%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}}) 一文中讲解了三个规则，但是后期发现还有其它内容需要调整，索性在这篇文章中统一整理下。

本文基于 Obsidian 的 Github Publisher 插件，版本：v6.15.5，插件不同可以略有差异，不过差异影响使用了，我就会更新该篇文章。

### File paths
{{< figure src="/images/b77c030f8dca8973a49878ab9afcd5a9.webp" caption="">}}
Property key 可以通过文章的属性设置上传的目录，例如我这配置：  
- 通过 dir 属性获取上传的目录，最终的上传路径为 `Root folder / Property key`，即 `content/{dir}`
- 如果 dir 属性没有设置则默认上传到 Default folder 目录下，即  content/posts  



## Content

{{< figure src="/images/6e71367e83816e828e0f2bb80c43bf11.webp" caption="">}}
每行后面都有个箭头，↑箭头表示插件应用其它配置之前替换，↓箭头之后替换。
### `[[Wikilinks]]` 转 `[MDlinks](links)`
开启 Content -> `[[Wikilinks]]` 转 `[MDlinks](links)` 菜单。

{{< figure src="/images/0c42f241aa2372d3f5d99c30a0494278.webp" caption="">}}

### 图片
在 Obsidian 中引用本地图片链接时是不需要路径的，而在 Hugo 中一般会把图片统一放在 /static/images 目录下。

因此在 Hugo 文章的引用方式路径为 `/images/图片名称`。

> 我图片在本地存储的，如果你有图床那这个配置也没那么重要

现在要做的就是给 Obsidian 的本地图片路径增加 /images 前缀，使用到了 Text replacer 正则，有两个方案：

#### 方案一
等插件应用其它配置之后再执行，这样图片格式会从：
- `![[图片名.后缀]]` 变成了 `![图片名](图片名.后缀)`
- `![[图片名.后缀|caption]]` 变成了 `![caption](图片名.后缀)`
- `![[图片名.后缀|200]]` 变成了 `![200](图片名.后缀)`，200 指定图片宽度像素。
然后再进行正则匹配。

```
匹配内容：/\]\(([^/]+?)\.(png|jpg|jpeg|webp|gif)/
替换：](/images/$1.$2
箭头：↓
```


**缺点**：无法很好支持图片的 caption，因为拿 `![]()` 中括号里的内容时候有三种情况，但是没发很好的区分。
#### 方案二
直接匹配如下的双链格式，而不是等转成 md 的链接格式：
- `![[图片名.后缀]]`
- `![[图片名.后缀|caption]]`
- `![[图片名.后缀|200]]`，对于目前我的图片尺寸我是使用 Image Converter 处理好的，所以不特别和 `![[图片名.后缀|caption]]` 情况区分，直接归位一类

```
匹配内容：/\!\[\[([^/]+?)\.(png|jpg|jpeg|webp|gif)\|?([^\|]*?)\]\]/
替换：{{< figure src="/images/$1.$2" caption="$3">}}
箭头：↑
```
**优点**：支持图片的 caption，但没有区分caption 和 width 的区别，这个可以做到的，不过个人觉得必要性还不大
**缺点**：不能支持 md 的图片链接格式，即 `!()[]`
### 内部文章引用
在 Obsidian 中文章的之间的引用使用 `[[文章名]]` 用法，而在 Hugo 中用的是 `{{< relref "文章名.md" >}}` 语法。

Text replacer 正则：
```
匹配内容：/\]\((.+?)\.md/
替换：]({{< relref "$1".md >}}
箭头：↓
```

在 obsidian 文章之间引用是没有 md 后缀的，因此执行该替换的时间是等文章使用插件转化后再执行，据此后箭头向下。

#### 锚点

如果文章链接中出现了锚点跳转，则记着关闭这个配置：
{{< figure src="/images/b626d0ca540a27e08f1ac99ed6bc6487.webp" caption="">}}

> 锚点跳转就是跳转到文章的某个标题处，在 Obsidian 的格式为 `[[文章#标题]]`

该配置会将锚点(`#标题`)处理：
1.  转小写
2. 空格转化减号(`-`)
虽然我测试对于这种转了的， Hugo 还是能访问到对应的标题，但我还是担心我测试不到位，直接关闭省事。

### 属性一级转二级
在 obsidian 中如果属性是二级的话，展示效果就非常不友好直观，所以我会在 obsidian 里这样写需要转二级的属性：
```
---
cover.image: 
---
```
这个属性我用来设置文章的封面，具体要看看自己的主题是不是这样，如果不是那需要自己调整下。

Text replacer 正则：
```
匹配内容：/cover\.image/
替换：cover:\n    image
箭头：↑
```
替换中二级的缩紧正常应该使用 `\t`，但我测试发现无效就改用 4 个空格。

###  标签
在 Obsidian 里是支持 `/` 方式渲染多级标签的，在 Hugo 中是不支持的，所以需要开启将 `/` 转位 `_`。
{{< figure src="/images/3c3e40b83ac5be7799d55696eadc7218.webp" caption="">}}

### 遇到的问题
Markdown hard line break 最好关闭，我这边开启造成了空行变多，使代码难看、表格无法渲染。

## Attachment & embed note config
默认Transfer attachments 会开启附件上传的，重点是需要使用 Default attachment folder 设置附近的上传目录。

{{< figure src="/images/18c2e020cbb2bb6142ea0611c3eb62da.webp" caption="">}}
