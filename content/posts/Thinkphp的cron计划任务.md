---  
date: 2017-08-26 11:14:38+08:00  
tags:   
title: Thinkphp的cron计划任务  
slug: thinkphp-cron  
share: true  
canonicalURL: https://www.jianshu.com/p/55c4bb65d9e8  
keywords:   
description:   
series:   
cover:  
  image: https://images.unsplash.com/photo-1667064098336-8578c2db7354?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwxM3x8Y3JvbnxlbnwwfDB8fHwxNzAzMzA1OTYxfDA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
首先明白一点，thinkphp 中的计划任务不是真正意义上的，它是使用一个文件的修改时间差来比较是否执行，并且必须依赖网站的访问才能触发脚本  
  
在 conf 下新建两个文件并配置 tags.php 和 crons.php  
  
![](/images/20231208091261.webp)  
  
配置 tag.php  
```php  
return array(  
    'app_end'=>array('\Behavior\CronRun'), // 定时任务  
);  
```  
配置 crons.php  
```php  
return array(  
    'cron_1'=>array('cron1', '10') //cron1要执行的脚本  
)  
````  
cron1 默认在 `ThinkPHP\Library\Cron\cron1.php` 如果没有 cron 目录则新建一个,cron1.php 自己所要执行的脚本  
  
**注意**：  
* app_end 的路径配置，`\Behavior\CronRun` 路径要包含 `\` 否则不被认为是 Behavior  
* 如果报错 Log::write 相关错误，则打开 `ThinkPHP\Library\Behavior\CronRunBehavior.class.php` 大约 55 行 `\Think\Log::write(implode('',$log));`  
  
