---  
date: 2016-06-01 22:06:11+08:00  
tags:   
title: 这种div高度自适应确定你知道吗？  
slug: div-height-auto  
share: true  
canonicalURL: https://www.jianshu.com/p/e50d1f11cf83  
keywords:   
description:   
series:   
---  
  
## 随子元素 div 高度自适应  
  
如何下面的 child1 和 child2 浮动，parent 高度就会为 0，前提 child1 和 child2 都有高度，如果你想 parent 的高度自适应，请继续阅读  
  
```html  
<body>  
	<div class="parent">  
		<div class="child1"></div>  
		<div class="child2"></div>  
	</div>  
</body>  
```  
* 样式省略  
### 方法 1  
```css  
.parent{  
	overflow:hidden;  
}  
```  
### 方法 2  
* 会使用到伪元素：after,如果不懂请看 [伪元素](http://www.jianshu.com/p/12f83956b231)  
```css  
.parent:after{  
	content: " ";  
	height: 0;  
	display: block;  
	clear: both;  
	visibility: hidden;  
}  
```  
  
***  
  
## 根据 div 宽度的百分比调整 div 高度  
  
假设我的 div 宽度会自动变化,但是我的高度只想设置成 div 宽度的一半,该如何实现呢?  
  
  
```html  
<body>  
	<div class="main"></div>  
</body>  
```  
  
设置 main 的高度为宽度的一半  
  
```css  
.main:after{  
	display: block;  
	content: ' ';  
	padding-top: 50%;  
	border: 1px solid black;/*便于看出div的效果*/  
}  
		  
```