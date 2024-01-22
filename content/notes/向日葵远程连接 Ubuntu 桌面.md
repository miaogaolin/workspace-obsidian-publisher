---
date: 2022-01-28T11:29:22+08:00
tags: 
title: 向日葵远程连接 Ubuntu 桌面
slug: "1705894162"
share: true
canonicalURL: 
keywords: 
description: 
series: 
lastmod: 
cover:
  image: 
author: 
dir: notes
---


如果安装后可以登录，但是马上又会断连，那就需要安装 lightdm。
{{< figure src="/images/f02006218c83cd4991bc1b9cc8fa0aa7.webp" caption="">}}
下来切换桌面管理。

```bash
sudo dpkg-reconfigure lightgm-dk-greeter 
```

切换后重启系统即可。