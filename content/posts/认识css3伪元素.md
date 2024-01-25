---  
date: 2016-06-01 22:10:21+08:00  
tags:   
title: 认识css3伪元素  
slug: css3-class  
share: true  
canonicalURL: https://www.jianshu.com/p/12f83956b231  
keywords:   
description:   
series:   
cover:  
  image: https://images.unsplash.com/photo-1524666643752-b381eb00effb?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwxMHx8Y3NzfGVufDB8MHx8fDE3MDMzMDYyNzl8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  

  
css2 的规定为单冒号表示，而 css3 已经明确表示伪类使用单冒号，伪元素使用双冒号，本次所有测试双冒号。
  

  
提示：对于读者阅读，有的地方显得有些冗余，但是，这是为了测试没办法了，如果写的不全面或者有错误，请您提出建议。
  

  
## ::selection
  
对用户所选取的部分样式改变
  

  
### 浏览器支持
  

  
| IE | FireFox| Chrome |Safari|Opera|Edge|360|
  
| :-: |:-:|:-:|:-:|:-:|:-:|:-:|
  
| 支持 |支持 |支持 | 没测试 | 支持 |   支持 | 支持 |  
  
注：IE9+、Opera、Google Chrome 以及 Safari 中支持 `::selection` 选择器。
  
Firefox 支持替代的 `::-moz-selection`。
  

  
### 代码示例
  

  
```html
  
<!DOCTYPE html>
  
<html lang="en">
  
<head>
  
	<meta charset="UTF-8">
  
	<title>Document</title>
  
	<style type="text/css">
  
		::selection{
  
			background-color: red;
  
		}
  
	</style>
  
</head>
  
<body>
  
	<article>
  
		::selection元素选择器的学习,如果选中显示为红色，默认为蓝色的
  
	</article>
  
</body>
  
</html> 
  
```
  
[w3cshool,css3 ::selection](http://www.w3school.com.cn/cssref/selector_selection.asp)
  
*** 
  
## ::before
  
在元素显示内容之前进行某些样式
  

  
###浏览器支持
  

  
| IE | FireFox| Chrome |Safari|Opera|Edge|360|
  
| :-: |:-:|:-:|:-:|:-:|:-:|:-:|
  
| 支持 |支持 |支持 |没测试|支持  |支持 |支持 |
  
IE9 包括 9 版本以上支持双冒号
  
###代码示例
  
```html
  
<!DOCTYPE html>
  
<html lang="en">
  
<head>
  
	<meta charset="UTF-8">
  
	<title>Document</title>
  
	<style type="text/css">
  
		li::before{
  
			content: "星期";
  
		}
  
	</style>
  
</head>
  
<body>
  
	<ul>
  
		<li>一</li>
  
		<li>二</li>
  
		<li>三</li>
  
		<li>四</li>
  
		<li>五</li>
  
	</ul>
  
</body>
  
</html>
  
```
  
[w3cshool,css :before](http://www.w3school.com.cn/cssref/selector_before.asp)
  
***
  
##::after
  
[项目中的应用]({{< relref "%E8%BF%99%E7%A7%8Ddiv%E9%AB%98%E5%BA%A6%E8%87%AA%E9%80%82%E5%BA%94%E7%A1%AE%E5%AE%9A%E4%BD%A0%E7%9F%A5%E9%81%93%E5%90%97.md" >}})
  

  
在元素显示内容之后进行某些样式内容操作
  
### 浏览器支持
  

  
| IE | FireFox| Chrome |Safari|Opera|Edge|360|
  
| :-: |:-:|:-:|:-:|:-:|:-:|:-:|
  
| 支持 |支持 |支持 |没测试|支持  |支持 |支持 |
  
IE9 包括 9 版本以上支持双冒号
  
### 代码示例
  
```html
  
<!DOCTYPE html>
  
<html lang="en">
  
<head>
  
	<meta charset="UTF-8">
  
	<title>Document</title>
  
	<style type="text/css">
  
		.sp::after{
  
			content: "，";
  
		}
  
	</style>
  
</head>
  
<body>
  
	<p>
  
		<span class="sp">before表示之前</span>
  
		<span class="sp">after表示之后</span>
  
		<span class="sp">这样说了和没说一样</span>
  
		<span >废话！</span>
  
	</p>
  
</body>
  
</html>
  
```
  

  
[w3cshool,css :after](http://www.w3school.com.cn/cssref/selector_after.asp)
  

  
***
  
## ::first-letter
  
对元素内容的第一字母进行样式操作
  

  
### 浏览器支持
  

  
| IE | FireFox| Chrome |Safari|Opera|Edge|360|
  
| :-: |:-:|:-:|:-:|:-:|:-:|:-:|
  
| 支持 |支持 |支持 |没测试|支持  |支持 |支持 |
  
IE9 包括 9 版本以上支持双冒号
  
### 代码示例
  
```html
  
<!DOCTYPE html>
  
<html lang="en">
  
<head>
  
	<meta charset="UTF-8">
  
	<title>Document</title>
  
	<style type="text/css">
  
		p::first-letter{
  
			color:red;
  
		}
  
	</style>
  
</head>
  
<body>
  
	<p>
  
		CSS 伪类用于向某些选择器添加特殊的效果。<br/>
  
		 CSS 伪元素用于将特殊的效果添加到某些选择器。
  

  
	</p>
  
</body>
  
</html>
  
```
  
[w3cshool,css :first-letter](http://www.w3school.com.cn/cssref/selector_first-letter.asp)
  

  
***
  
## ::first-line
  
对元素内容的第一行进行样式操作
  

  
### 浏览器支持
  

  
| IE | FireFox| Chrome |Safari|Opera|Edge|360|
  
| :-: |:-:|:-:|:-:|:-:|:-:|:-:|
  
| 支持 |支持 |支持 |没测试|支持  |支持 |支持 |
  
IE9 包括 9 版本以上支持双冒号
  
### 代码示例
  
```html
  
<!DOCTYPE html>
  
<html lang="en">
  
<head>
  
	<meta charset="UTF-8">
  
	<title>Document</title>
  
	<style type="text/css">
  
		p::first-line{
  
			color:red;
  
		}
  
	</style>
  
</head>
  
<body>
  
	<p>
  
		CSS 伪类用于向某些选择器添加特殊的效果。<br/>
  
		 CSS 伪元素用于将特殊的效果添加到某些选择器。
  

  
	</p>
  
</body>
  
</html>
  
```
  
[w3cshool,css :first-line](http://www.w3school.com.cn/cssref/selector_first-line.asp)
  
***
  
## 总结
  
这次只是对 css3 规定的双冒号进行测试，在主流浏览器上双冒号都可以实现。如果读者您想有更好的兼容性，我建议还是使用单冒号，因为从上面可以看出对于 IE9 以下都不兼容，但是对于这批用户也占了相当一部分。