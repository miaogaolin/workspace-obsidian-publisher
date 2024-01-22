---
date: 2022-01-27T11:26:09+08:00
tags: 
title: Ubuntu 安装 ssh 服务
slug: ubuntu-ssh
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


## 安装

```bash
sudo apt install openssh-server
```

安装时出现了问题：

```bash
无法解析域名 “cn.archive.ubuntu.com”
```

解决办法：

```bash
# 打开
$ vi /etc/resolv.conf

# 修改内容
nameserver 8.8.8.8
```

下来就可以正常远程登录了

```bash
ssh 用户名@ip
```

## 启动

如果停止了，使用下面命令启动。

```bash
systemctl start ssh
```

## 参考

- [apt-get 提示 无法解析域名“cn.archive.ubuntu.com” 的解决_wuzhidefeng的博客-CSDN博客](https://blog.csdn.net/wuzhidefeng/article/details/80540188)