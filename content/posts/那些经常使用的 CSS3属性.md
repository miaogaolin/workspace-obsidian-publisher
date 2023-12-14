---  
date: 2016-05-31 19:44:16+08:00  
tags:   
title: 那些经常使用的 CSS3属性  
slug: css-property  
share: true  
canonicalURL: https://www.jianshu.com/p/536f01ce8a9c  
keywords:   
description:   
series:   
---  
  
## 1、display:flex||inline-flex  
|display|flex|inline-flex|  
|------|-----|-----|  
|解释|将对象作为弹性伸缩盒显示|将对象作为内联块级弹性伸缩盒显示|  
  
### 项目中的应用  
  
我当时写过一个因为子元素浮动让 div 自适应高度的解决办法，使用的是 css 方法解决的。[div高度自适应](http://www.jianshu.com/p/e50d1f11cf83)  
下面就是换用 display:flex 解决  
  
```html  
<!DOCTYPE html>  
<html lang="en">  
<head>  
	<meta charset="UTF-8">  
	<title>Test</title>  
  <style type="text/css">  
    .main{  
      width:200px;  
      background-color: red;  
      display: flex;/*父div设置该属性*/  
    }  
    .main>div{  
      width: 50px;  
      height: 50px;  
      border: 1px solid blue;  
      box-sizing: border-box;/*这是css3属性，如果不懂，请继续往下阅读*/  
      /*float:left;这个属性就不需要了，会自动浮动*/  
    }  
  </style>  
</head>  
<body>  
  <div class="main">  
    <div>1</div>  
    <div>2</div>  
    <div>3</div>  
    <div>4</div>  
  </div>  
</body>  
</html>  
```  
display:inline-flex,如果想看到效果，将上面的 display:flex,换成 display:inline-flex,并且将 width:200px 删除。在没有测试之前，有的人可能会认为.main 会占据整个一行，但是，测试结果是，它会根据子元素所有的 div 大小自适应宽度和高度  
  
***  
## 2、属性 align-items  
[项目中的应用]({{< relref "%E5%AE%9E%E7%8E%B0HTML%E5%85%83%E7%B4%A0%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E7%9A%84%E5%85%AD%E7%A7%8D%E6%96%B9%E6%B3%95.md" >}})  
  
|属性值|解释|  
|--|-----|  
|flex-start|弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界|  
|flex-end|弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界|  
|center|弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度|  
|baseline|如弹性盒子元素的行内轴与侧轴为同一条，则该值与 'flex-start' 等效。其它情况下，该值将参与基线对齐|  
|stretch|如果指定侧轴大小的属性值为 'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照 'min/max-width/height' 属性的限制|  
  
 再次注意:  
   
* stretch，子元素设置高度为 auto,不能固定高度才能在父容器中被拉伸  
  
![20231207171266.webp](/images/20231207171266.webp)  
![这里写图片描述](/images/20231207171269.webp)  
  
## 3、属性 flex-flow  
* 注意: 当父容器设置了 display:flex||inline-flex,这个属性才起作用。  
* 解释: 设置弹性元素的排列和是否换行  
  
flex-flow：<' flex-direction '> || <' flex-wrap '>  
- <' flex-direction '>：定义弹性盒子元素的排列方向。  
-  <' flex-wrap '>：控制 flex 容器是单行或者多行。  
  
|属性|值|  
|-------|-------|  
|flex-direction|row(默认),row-reverse,column,column-reverse,initial,inherit|  
|flex-wrap|nowrap(默认),wrap,wrap-reverse,initial,inherit|  
  
* initial,原本元素的默认值，也就是不使用该 css3 属性的值  
* 注意：Internet Explorer 或 Opera 15 及其之前的版本不支持 initial 关键字作为一个属性值。  
  
```html  
.main{  
  background-color: red;  
  width: 50px;  
  display: flex;/*这个是前提*/  
  flex-flow：<' flex-direction '> || <' flex-wrap '>  
}  
```  
![20231207171266.webp](/images/20231207171266.webp)  
***  
  
## 4、属性 box-sizing  
  
|值|解释|  
|----|----|  
|content-box|(默认) 设置的 border 和 padding 值，向宽度和高度外增加|  
|border-box|设置的 border 和 padding 值，向宽度和高度内增加|  
  
- box-zizing: border-box,不会影响原元素的高度与宽度  
-  *box-sizing：border-box,如果想在一个 div 中放多个图片并且平均分配宽度，如果不设置这个属性图片就会全部充满这个行，现在就可以使用这个属性很好的解决  
  
***  
  
## 5、transition  
  
通过 css3 定义简单的缓慢动画效果,下面是 transition 的四个复合属性  
* transition-property 规定设置过渡效果的 CSS 属性的名称。  
* transition-duration 规定完成过渡效果需要多少秒或毫秒。  
* transition-timing-function 规定速度效果的速度曲线  
* transition-delay 定义过渡效果何时开始。  
  
* 这里我不做太多详细的解释，只对实战中的应用进行演示 [详情](http://www.w3school.com.cn/cssref/pr_transition.asp)  
  
```html  
<!DOCTYPE html>  
<html lang="en">  
<head>  
	<meta charset="UTF-8">  
	<title>Test</title>  
  <style type="text/css">  
    .block{  
      width: 80px;/*初始宽度*/  
      height: 80px;  
      background-color: blue;  
      opacity: 1;/*初始透明度*/  
    }  
    .block:hover{  
      transition: all 1s;/*单个属性transition: opacity 1s,如果是多个的话就是all*/  
      opacity: 0;/*移上去时候的透明度*/  
      width: 150px;/*移上去时候的宽度*/  
    }  
  </style>  
</head>  
<body>  
  <div class="block"></div>  
</body>  
</html>  
```  
  
让一个 80*80px 的方块，在 1s 内宽度由 80px 到 150px,并且透明度由 1 变成 0  
如果还想添加别的属性，只需在.block 中设置初始的属性，在.block:hover 中设置要达到的最终值  
  
##6、总结  
css3 有很多属性都特别好用，这是我知道的几个实用属性，后期发现好的也会增加进来。希望大家有建议提出来!