---  
date: 2016-06-11 21:03:33+08:00  
tags:   
title: 使用css3属性处理单词的换行和断词  
slug: css3-word  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
---  
  
* 问题呈现  
  
```html  
<div class="main">  
Nameofuser-definedcallbackfunctiontobecalledwheneverastreamtriggersanotification.   
</div>  
```  
  
```css  
<style type="text/css">  
.main{  
	width: 100px;  
	border: 2px solid red;  
}  
</style>  
```  
![](/images/20231208091256.webp)  
  
> 默认情况下,连续的单词如果在一行容纳不下的话会在空格和连字符处换行，那如何让它换行呢?  
  
* 认识 word-break 属性  
  
|属性值|解释|  
|----|-----|  
|normal|使用浏览器默认的换行规则 (默认)|  
|break-all|允许在单词内换行|  
|keep-all|只能在半角空格或连字符处换行|  
  
> 现在大多说的浏览器默认的换行规则为半角空格和连字符,因此 normal 和 keep-all 的效果是一样的。  
  
* word-break: break-all  
  
```css  
<style type="text/css">  
.main{  
	width: 100px;  
	border: 2px solid red;  
        word-break: break-all;  
}  
</style>  
```  
  
![](/images/20231208091266.webp)  
  
>  word-break: break-all，打破了默认的换行规则。那如果想保留空格和连字符处换行怎么办？  
  
* 认识 word-wrap 属性  
  
|属性值|解释|  
|------|-------|  
|normal|使用浏览器默认的换行规则 (默认)|  
|break-word|长单词进行换行|  
  
> 下来看一下演示,我把单词内部插入了几个空格  
  
  
* 先看默认的，以作对比。我把长单词多插入了几个空格，以便效果明显  
  
```html  
<div class="main">  
	Name of user-definedcallbackfunction to be called wheneverastreamtriggersanotification.   
</div>  
```  
  
```css  
<style type="text/css">  
.main{  
	width: 100px;  
	border: 2px solid red;  
}  
</style>  
```  
  
![](/images/20231208091273.webp)  
  
> 默认情况下,图上标号 2 和 4 是连续长单词，中间没有空格和连字符，所以没有换行 (溢出)。  
  
* 下来看看 word-wrap: break-word 演示  
  
```css  
<style type="text/css">  
.main{  
	width: 100px;  
	border: 2px solid red;  
    word-wrap: break-word;  
}  
</style>  
```  
  
![](/images/20231208091277.webp)  
  
> 从图上看,保留了空格和连字符的换行状态。只是将前面图上标号 2 和 4 行的长单词进行了换行。  
  
## 总结  
  
* word-break: break-all, 打破了浏览器的默认换行规则  
* word-wrap: break-word, 保留浏览器的默认换行规则，一旦一个连续长单词一行容纳不下，就只对这个长单词进行打破换行