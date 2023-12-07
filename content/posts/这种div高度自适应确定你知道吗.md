---  
date: 2016-06-01 22:06:11 +8000  
tags:   
title: 这种div高度自适应确定你知道吗？  
slug: div-height-auto  
share: true  
canonicalURL: https://www.jianshu.com/p/e50d1f11cf83  
keywords:   
description:   
series:   
---  
  
# 随子元素div高度自适应  
  
如何下面的child1和child2浮动，parent高度就会为0，前提child1和child2都有高度，如果你想parent的高度自适应，请继续阅读  
  
```html  
<body>  
	<div class="parent">  
		<div class="child1"></div>  
		<div class="child2"></div>  
	</div>  
</body>  
```  
* 样式省略  
## 方法1  
```css  
.parent{  
	overflow:hidden;  
}  
```  
## 方法2  
* 会使用到伪元素：after,如果不懂请看[伪元素](http://www.jianshu.com/p/12f83956b231)  
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
  
# 根据div宽度的百分比调整div高度  
  
假设我的div宽度会自动变化,但是我的高度只想设置成div宽度的一半,该如何实现呢?  
  
  
```html  
<body>  
	<div class="main"></div>  
</body>  
```  
  
设置main的高度为宽度的一半  
  
```css  
.main:after{  
	display: block;  
	content: ' ';  
	padding-top: 50%;  
	border: 1px solid black;/*便于看出div的效果*/  
}  
		  
```