---
date: 2022-01-26T11:10:57+08:00
tags: 
title: Ubuntu 开发所有端口
slug: ubuntu-port
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


## 指定端口

防火墙增加端口。
```bash
# 查看端口列表
sudo ufw status
  
# 增加
sudo ufw allow 7890

# 重启
sudo ufw reload
```

  

## 开放所有

三步骤：
1. 关闭 ufw
2. 开放 iptables 所有规则
3. 检查端口绑定的 ip
### 1. 关闭 ufw

```bash
sudo ufw disable
```

### 2. 开放 iptables

```bash
iptables -F
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
```

- `F`：删除所有策略链
- `X`：删除用户定义的链
- `P INPUT/OUTPUT/FORWARD` ：接受指定的流量

### 3. 检查 ip

先查看端口情况。
```bash
netstat -anp | grep 7890
```

结果中如果看到了 `:::7890` 那说明对外开放，倘若是 `127.0.0.1:7890` 那就只针对当前电脑开放。
  
这种情况就要不同情况不同解决办法。

## 参考

- [如何暂时禁用 iptables 防火墙 - 知乎](https://zhuanlan.zhihu.com/p/38706862)