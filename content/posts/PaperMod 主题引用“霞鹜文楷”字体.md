---  
date: 2023-12-20T15:39:56+08:00  
tags:   
title: PaperMod 主题引用“霞鹜文楷”字体  
slug: lxgw-font  
share: true  
canonicalURL:   
keywords:  
  - hugo  
  - papermod  
  - 霞鹜文楷  
description:   
series:   
lastmod:   
cover:  
  image: /images/cf1fc10b763df1f061cc4608337a6cc4.webp  
author:   
---  
  
  
![cf1fc10b763df1f061cc4608337a6cc4.webp](/images/cf1fc10b763df1f061cc4608337a6cc4.webp)  
有两种方法，一种是 [下载字体]([lxgw/LxgwWenKai](https://github.com/lxgw/LxgwWenKai)) 并上传到自己的项目中引用，一种是使用现成的 CDN 引用，我直接选择了 CDN 方式。  
  
下来替换主题的默认模板和样式，进入 Hugo 内容管理的仓库根目录：  
1. 新建模板 layouts/partials/extend_head.html，设置内容：  
```html  
<link rel="stylesheet" href="https://cdn.staticfile.org/lxgw-wenkai-screen-webfont/1.7.0/style.css" media="print" onload="this.media='all'">  
```  
这是七牛云的 CDN，更多的引用方式可以看看这个项目：[chawyehsu/lxgw-wenkai-webfont](https://github.com/chawyehsu/lxgw-wenkai-webfont?tab=readme-ov-file)  
  
2. 全局引用字体，新建 assets/css/extended/fonts.css，设置内容：  
```css  
body,  
.post-content code {  
  font-family: "LXGW WenKai Screen", "Roboto", "PingFang SC", "Microsoft Yahei", sans-serif;  
}  
```  
  
设置完成，不过再扩展下里面涉及到的知识点：  
1. 异步加载 CSS，设置 link 标签属性  
	- `media="print"` 在打印的时候加载该引用  
	- `onload="this.media='all'"` 等页面加载完后在将 media 设置回去，即默认，这就会开始应用该 css  
	- media 属性的详细用法请看：[media types](https://developer.mozilla.org/en-US/docs/Web/CSS/@media#media_types)  
1. 在该 cdn 引用的 css 文件中，会有这么一个设置：  
``` css  
@font-face {  
  font-display: swap;  
}  
```  
`font-display` 表示加载 `font-family` 后面字体的规则：  
- swap 如果想使用的字体没有，则直接使用备用字体，等字体下载完成再交换渲染  
- auto 这是默认，一般会先文本空白等字体下载   
更多解释，可以参考下这篇： [Web 性能优化：使用 CSS font-display 控制字体加载和替换](https://zxuqian.cn/css-font-display-intro/)  
