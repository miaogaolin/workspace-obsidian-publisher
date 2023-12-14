---  
date: 2016-06-05 12:42:47+08:00  
tags:   
title: php缓冲区与header函数之间的秘密  
slug: php-header  
share: true  
canonicalURL: https://www.jianshu.com/p/09262e2c9088  
keywords:   
description:   
series:   
---  
  
我们在实际的开发中,是否听说过在 header 之前不能有任何的实际输出。甚至有的认为 header 函数必须写在代码的最前面。可是你是否试验过 header 函数之前输出东西?下来让我们更深层次的了解一下  
***  
* 测试 header 之前有输出  
```php  
<?php  
echo 'hello world!';  
header('content-type: text/html;charset=utf-8;');  
```  
我经过测试时可以成功的,不会出现任何错误和警告。不知道你们是怎么样的?可是我想大多说都是没有问题,如果出现了*Cannot modify header information - headers already sent*这样的警告,这是是说不能修改头部信息,头部信息已经发送。下来就了解一下为什么会出现两种不同结果?  
  
# 缓冲区  
  
做个比喻,就好比我们看电影时的缓存一样。它不会之间立即给我们播放出来,而是先将一部分下载好的电影放到缓存里面,再有缓存播放出来。我们编写 php 代码也是这个道理  
  
## php 的缓存机制 -output_buffering  
  
* php 中的常用 ob 函数  
  
|函数|解释|  
|-----|------|  
|ob_start|打开输出缓冲区|  
|ob_clean|清空缓冲区|  
|ob_get_contents|返回缓冲区内容|  
|ob_get_clean|返回缓冲区内容,并清空|  
  
```php  
<?php  
ob_start();  
echo 'hello world!';  
echo ob_get_contents();//输出hello world!hello world!  
```  
  
* 在 php.ini 配置文件中,修改缓冲区大小  
![](http://upload-images.jianshu.io/upload_images/2031034-3ffe7941ffa5d06f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
  
注：一般在 233 行左右,默认是 4096 表示 4096 字节也就是 4kB。  
  
下来将 4096 修改为 5,重新运行这段代码  
```php  
<?php  
echo 'hello world!';  
header('content-type: text/html;charset=utf-8;');  
```  
对于刚才测试不会出现警告或错误的现在却出现了,错误就是:**Cannot modify header information - headers already sent**  
  
## header 与缓冲区之间的分析  
  
1. 为什么我们之前说 header 之前不能有输出？  
  
对于 header 函数,它是像客户端发送原始的 http 报头,是声明我们所写网页到底是什么内容,所以一但这个声明之前有内容就是错误的,是不符合 http 规则的  
  
2. 下来说说 php 中的 header  
  
在 php 中 header 是不经过缓冲区的,它会经过服务器直接输出到客户端  
  
3. 解释之前的警告 Cannot modify header information  
  
当我们在 header 之前写了一些输出的话,它会先经过缓冲区。因此即便你写的了前面,最终的输出顺序还是先 header 在 echo。  
  
可是一但我们输出的内容缓存区放不下,即之前的输出 'hello world!' > 5 个字节。就会直接输出出来,也就是这样先输出 'hello world' 再 header(...),这样就违背了真实的 header 之前不能有输出  
  
# 总结  
在实际当中,我们最好还是把 header 写在页面最前面。因为我们就不确定我们 header 之前的输出内容是否缓冲区能放下。