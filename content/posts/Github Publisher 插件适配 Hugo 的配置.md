---
date: 2024-01-14T18:14:22+08:00
tags: 
title: Github Publisher 插件适配 Hugo 的配置
slug: github-publisher-hugo
share: true
canonicalURL: 
keywords: 
description: 
series: 
lastmod: 2024-02-05T09:35:00
cover:
    image: https://images.unsplash.com/photo-1653402438643-b230db019d27?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw0NHx8aHVnb3xlbnwwfDB8fHwxNzA1MjI3MjkzfDA&ixlib=rb-4.0.3&q=80&w=400
---
先写了 [使用 Obsidian 免费建个人博客]({{< relref "%E4%BD%BF%E7%94%A8%20Obsidian%20%E5%85%8D%E8%B4%B9%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}}) 这篇文章，但是发现枯燥的讲解这个插件的配置不适合初学者，所以索性在这篇文章中统一整理下，对于想更深入了解的人可以选择性看看这篇。

本文基于 Obsidian 的 Github Publisher 插件，版本：v6.14.2，版本不同可能略有差异，如果差异影响使用了，我就会更新该篇文章，及时收到通知可以关注[我的频道](https://t.me/evan_share)。

下面我会对重要的配置进行讲解，如果你暂时不想看了，可以使用我提供的[默认配置](https://github.com/miaogaolin/obsidian-github-publisher-hugo)。

## File paths
![b77c030f8dca8973a49878ab9afcd5a9.webp](/images/b77c030f8dca8973a49878ab9afcd5a9.webp)
Property key 可以通过文章的属性设置上传的目录，例如我这配置：  
- 通过 dir 属性获取上传的目录，最终的上传路径为 `Root folder / Property key`，即 `content/{dir}`
- 如果 dir 属性没有设置则默认上传到 Default folder 目录下，即 content/posts  


## Content

{{< figure src="/images/6e71367e83816e828e0f2bb80c43bf11.webp" caption="Text replacer" width="" height="">}}
每行后面都有个箭头，↑箭头表示插件应用其它配置之前替换，↓箭头表示之后替换。
### `[[Wikilinks]]` 转 `[MDlinks](links)`
开启 Content -> `[[Wikilinks]]` 转 `[MDlinks](links)` 菜单。

![0c42f241aa2372d3f5d99c30a0494278.webp](/images/0c42f241aa2372d3f5d99c30a0494278.webp)

### 图片
在 Obsidian 中引用本地图片链接时是不需要路径的，而在 Hugo 中一般会把图片统一放在 /static/images 目录下。

因此在 Hugo 文章的引用方式路径为 `/images/图片名称`。

> 我图片在本地存储的，如果你有图床那这个配置也没那么重要

现在要做的就是给 Obsidian 的本地图片路径增加 /images 前缀，使用到了 Text replacer 正则。

支持 `![]()` 的图片格式，箭头 ↓ 表示会等 `![[]]` 转化成 `![]()` 之后执行：
```
匹配内容：/\]\(([^/\)]+?)\.(png|jpg|jpeg|webp|gif)/
替换：](/images/$1.$2
箭头：↓
```
支持 `![[]]` 的图片格式，且增加了 caption 和 size 的支持，分两种情况：
```
情况一，支持 ![[filename.png|size]] 
匹配内容：/\!\[\[([^/\]]+?)\.(png|jpg|jpeg|webp|gif)\|(\d+)(x(\d+))?\]\]/
替换：{{</* figure src="/images/$1.$2"  width="$3" height="$5"*/>}}
箭头：↑

情况二，支持 ![[filename.png|caption|size]]
匹配内容：/\!\[\[([^/\]]+?)\.(png|jpg|jpeg|webp|gif)\|([^\|]*?)(\|(\d+)(x(\d+))?)?\]\]/
替换：{{</* figure src="/images/$1.$2" caption="$3" width="$5" height="$7"*/>}}
箭头：↑
```
上面的两种图片格式都支持，区别就是 `![[]]` 格式会支持图片的 caption 和 size。
### 内部文章引用
在 Obsidian 中文章的之间的引用使用 `[[文章名]]` 用法，而在 Hugo 中用的是 `{{</* relref "文章名.md" */>}}` 语法。

Text replacer 正则：
```
匹配内容：/\]\(([^)\.]+)\.md/
替换：]({{</* relref "$1".md */>}}
箭头：↓
```

在 Obsidian 文章之间引用是没有 md 后缀的，因此执行该替换的时间是等文章使用插件转化后再执行，据此后箭头向下。

### 文章锚点

如果文章链接中出现了锚点跳转，则记着下拉框选择 Lower：
![6eb9e895a11107c864eeb7a31509946a.webp](/images/6eb9e895a11107c864eeb7a31509946a.webp)

> 锚点跳转就是跳转到文章的某个标题处，在 Obsidian 的格式为 `[[文章#标题]]`

该配置会将锚点 (`#标题`) 处理：
1.  转小写
2. 空格转化减号 (`-`)

只有转化才能正常访问锚点。

### 属性一级转二级
在 Obsidian 中如果属性是二级的话，展示效果就非常不友好直观，所以我会在 Obsidian 里这样写需要转二级的属性：
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
![3c3e40b83ac5be7799d55696eadc7218.webp](/images/3c3e40b83ac5be7799d55696eadc7218.webp)

### 踩的坑
Markdown hard line break 最好关闭，我这边开启造成了空行变多，使代码难看、表格无法渲染。

## Attachment & embed note config
默认 Transfer attachments 会开启附件上传的，重点是需要使用 Default attachment folder 设置附近的上传目录。

![18c2e020cbb2bb6142ea0611c3eb62da.webp](/images/18c2e020cbb2bb6142ea0611c3eb62da.webp)
## Issues
这是我给作者提交的问题，现在都已经修复。
- [正则表达式替换内容不支持转义字符](https://github.com/ObsidianPublisher/obsidian-github-publisher/issues/254)
- [锚点不支持中文](https://github.com/ObsidianPublisher/obsidian-github-publisher/issues/285)
- [Obsidian属性或FrontMatter重复](https://github.com/ObsidianPublisher/obsidian-github-publisher)