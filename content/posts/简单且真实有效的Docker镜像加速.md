---  
title: 简单且真实有效的Docker镜像加速  
date: 2021-07-21T17:39:52+08:00  
share: true  
keywords:  
  - docker加速  
description: docker镜像加速,docker加速配置  
slug: docker-speed  
---  
  
  
## 起因  
在使用Docker时，拉取镜像很慢，自己一番搜索后，找到很多文章都提供了好多办法，但很多的镜像地址使用后，都没有感觉很快。  
  
因此我就提供一个绝对可以加速的镜像地址给大家，如果出现无效可以联系我，我再更新。  
  
## 镜像地址  
  
阿里云的镜像：`https://wiim33kg.mirror.aliyuncs.com`  
  
注：根据自己的系统配置，如果不懂，在下方找到自己的系统配置。  
## Windows  
点击上方的设置>配置json  
![20231208111286.webp](/images/20231208111286.webp)  
配置完重启。  
  
## Mac  
在任务栏点击 Docker Desktop 应用图标 -> Perferences，在左侧导航菜单选择 Docker Engine，在右侧输入栏编辑 json 文件。  
  
将 `https://wiim33kg.mirror.aliyuncs.com` 加到"registry-mirrors"的数组里。  
  
点击 Apply & Restart按钮，等待Docker重启并应用配置的镜像加速器。  
## Linux  
  
```shell  
sudo mkdir -p /etc/docker  
sudo tee /etc/docker/daemon.json <<-'EOF'  
{  
  "registry-mirrors": ["https://wiim33kg.mirror.aliyuncs.com"]  
}  
EOF  
sudo systemctl daemon-reload  
sudo systemctl restart docker  
```