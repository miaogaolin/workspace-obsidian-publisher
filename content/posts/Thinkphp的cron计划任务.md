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
---

首先明白一点，thinkphp中的计划任务不是真正意义上的，它是使用一个文件的修改时间差来比较是否执行，并且必须依赖网站的访问才能触发脚本

在conf下新建两个文件并配置tags.php和crons.php

![](/images/20231208091261.webp)

配置tag.php
```php
return array(
    'app_end'=>array('\Behavior\CronRun'), // 定时任务
);
```
配置crons.php
```php
return array(
    'cron_1'=>array('cron1', '10') //cron1要执行的脚本
)
````
cron1默认在`ThinkPHP\Library\Cron\cron1.php`如果没有cron目录则新建一个,cron1.php自己所要执行的脚本

**注意**：
* app_end的路径配置，`\Behavior\CronRun`路径要包含`\`否则不被认为是Behavior
* 如果报错Log::write相关错误，则打开`ThinkPHP\Library\Behavior\CronRunBehavior.class.php`大约55行`\Think\Log::write(implode('',$log));`

