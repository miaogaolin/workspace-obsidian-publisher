---  
date: 2016-05-31 19:40:00+08:00  
tags:   
title: 实现HTML元素垂直居中的六种方法  
slug: css-center  
share: true  
canonicalURL: https://www.jianshu.com/p/af8c15dbc140  
keywords:   
description:   
series:   
---  
  
  
## 一、 img的垂直水平居中  
使用到的重要样式属性display,vertical-align  
vertical-align:middle这个属性是对table元素垂直居中起作用，如果想使用在img元素上，就注意下面的display设置  
  
```html  
<!DOCTYPE html>  
<html lang="en">  
<head>  
	<meta charset="UTF-8">  
	<title>Document</title>  
  <style type="text/css">  
    .main{  
      width: 400px;  
      height: 400px;  
      background-color: #aaa;  
      display: table;/*父元素设置表格属性*/  
      text-align: center;  
    }  
    .main span{  
      display: table-cell;/*img设置成表格元素属性*/  
      vertical-align: middle;/*两个display设置后这个属性就起作用*/  
    }  
  </style>  
</head>  
<body>  
    <div class="main">  
      <span><img src="1.jpg" alt="/" width="150px"></span>  
    </div>  
</body>  
</html>  
```  
注意: display:table-cell，这是对类似文字元素起作用的,所以包含在span标签内  
*对于文字居中也h1,span,p等类似文字标签都可以这样设置居中  
  
***  
## 二、 div的垂直水平居中  
这种方法同样适用于img,只需将child换成img就行，不再需要span了  
  
```html  
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Document</title>  
  <style type="text/css">  
    .main{  
      width: 400px;  
      height: 400px;  
      background-color: #aaa;  
      position: relative;  
    }  
    .child{  
      width: 200px;  
      height: 200px;  
      background-color: rgb(39,40,34);  
      position: absolute;  
      margin: auto;  
      left: 0;  
      right: 0;  
      top: 0;  
      bottom: 0;  
    }  
  </style>  
</head>  
<body>  
    <div class="main">  
      <div class="child"></div>  
    </div>  
</body>  
</html>  
```  
  
##  三、简便实现大部分元素的垂直居中  
  
水平居中，如果是文本(内联元素)text-align:center,div(块级元素)margin:0 auto;,，所以我就不写水平居中了，别嫌我懒哦  
  
* 会使用到属性[display:flex](http://blog.csdn.net/sinat_32124195/article/details/50760597)和[align-items](http://blog.csdn.net/sinat_32124195/article/details/50760597)  
* 我大概说一下，display:flex,将对象作为弹性伸缩盒显示  
* align-items:定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式。  
  
```html  
.main{/*给父容器设置*/  
display:flex;  
align-items:center;/*所有子元素都垂直居中了*/  
}  
```  
***  
## 四、使用css3 属性transform  
```html  
 transform: translateY(50%);/*给子元素设置*/  
```  
  
* transform: translateX(50%)也可以水平居中，但是上面已经说了，可以使用margin: 0 auot(块级),text-align(内联)，水平垂直居中transform: translateX(50%) translateY(50%);  
  
上面的所有垂直居中优点:是根本不需要知道父元素和子元素的尺寸,那下来下面的方法需要知道尺寸，但是不是不好，有的地方使用可能会很方便，看你项目中的情况  
  
## 五、单行文本的垂直居中  
  
* 设置文字的`line-height==父元素的height`  
```html  
<!DOCTYPE html>  
<html lang="en">  
<head>  
  <meta charset="UTF-8">  
  <title>Test</title>  
  <style type="text/css">  
    .block{  
      height: 80px;  
      background-color: blue;  
      line-height: 80px;/*值与父元素高度相等*/  
      text-align: center;  
    }  
  </style>  
</head>  
<body>  
  <div class="block">单行文本垂直居中</div>  
</body>  
</html>  
```  
## 六、需要知道子元素的尺寸  
  
实现 水平与垂直居中。  
```html  
/*省略了尺寸的设置，侧重了重点，读者可以把部分内容加上*/  
.main{/*父元素*/  
  position: relative;  
}  
.child{/*子元素*/  
	position: absolute;  
	top: 50%;  
	left: 50%;  
	margin-left: /*负的自身宽度一半*/  
	margin-top: /*负的自身高度一半*/  
}  
```  
## 七、总结  
以上的六种方法的兼容性我没有一一测试，如果读者有好的意见，希望您提出来，谢谢  
