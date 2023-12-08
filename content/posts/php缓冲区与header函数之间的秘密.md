---  
date: 2023-12-08 13:32:00 +8000  
tags:   
title: php缓冲区与header函数之间的秘密  
slug: php-header  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
---  
  
我们在实际的开发中,是否听说过在header之前不能有任何的实际输出。甚至有的认为header函数必须写在代码的最前面。可是你是否试验过header函数之前输出东西?下来让我们更深层次的了解一下  
***  
* 测试header之前有输出  
```php  
<?php  
echo 'hello world!';  
header('content-type: text/html;charset=utf-8;');  
```  
我经过测试时可以成功的,不会出现任何错误和警告。不知道你们是怎么样的?可是我想大多说都是没有问题,如果出现了*Cannot modify header information - headers already sent*这样的警告,这是是说不能修改头部信息,头部信息已经发送。下来就了解一下为什么会出现两种不同结果?  
  
# 缓冲区  
  
做个比喻,就好比我们看电影时的缓存一样。它不会之间立即给我们播放出来,而是先将一部分下载好的电影放到缓存里面,再有缓存播放出来。我们编写php代码也是这个道理  
  
## php的缓存机制-output_buffering  
  
* php中的常用ob函数  
  
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
  
* 在php.ini配置文件中,修改缓冲区大小  
![](http://upload-images.jianshu.io/upload_images/2031034-3ffe7941ffa5d06f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
  
注：一般在233行左右,默认是4096表示4096字节也就是4kB。  
  
下来将4096修改为5,重新运行这段代码  
```php  
<?php  
echo 'hello world!';  
header('content-type: text/html;charset=utf-8;');  
```  
对于刚才测试不会出现警告或错误的现在却出现了,错误就是:**Cannot modify header information - headers already sent**  
  
## header与缓冲区之间的分析  
  
1. 为什么我们之前说header之前不能有输出？  
  
对于header函数,它是像客户端发送原始的http报头,是声明我们所写网页到底是什么内容,所以一但这个声明之前有内容就是错误的,是不符合http规则的  
  
2. 下来说说php中的header  
  
在php中header是不经过缓冲区的,它会经过服务器直接输出到客户端  
  
3. 解释之前的警告Cannot modify header information  
  
当我们在header之前写了一些输出的话,它会先经过缓冲区。因此即便你写的了前面,最终的输出顺序还是先header在echo。  
  
可是一但我们输出的内容缓存区放不下,即之前的输出'hello world!' > 5个字节。就会直接输出出来,也就是这样先输出'hello world'再header(...),这样就违背了真实的header之前不能有输出  
  
# 总结  
在实际当中,我们最好还是把header写在页面最前面。因为我们就不确定我们header之前的输出内容是否缓冲区能放下。