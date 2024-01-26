---
date: 2017-11-17 11:57:08+08:00
tags: 
title: css的高级用法
slug: css-plus
share: true
canonicalURL: https://www.jianshu.com/p/eee2febb60bb
keywords: 
description: 
series: 
cover:
  image: https://images.unsplash.com/photo-1632882765546-1ee75f53becb?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw2MXx8Y3NzfGVufDB8MHx8fDE3MDMzMDY0Nzh8MA&ixlib=rb-4.0.3&q=80&w=400
---


##  模糊背景图片 (:before)

类似这样的效果
### 之前
![](/images/20231208091283.webp)

### 之后

![](/images/20231208091289no1.webp)


**重点注意: 颜色的变化,之后的图片相比之前的好像更暗淡一些**

```html
<div class="banner"></div>
```
> banner 放置类似上面的图片

```css
.banner{
    width: 800px;
    height: 400px;
    position: relative;
    background: url(图片路径) no-repeat;
    background-size: cover;
}

.banner:before{
	content: ' ';
	display: block;
	width: 100%;
	height: 100%;
	left: 0;
	top: 0;
	background: url(images/overlay.png);
	position: absolute;
	opacity: 0.5;
}
```