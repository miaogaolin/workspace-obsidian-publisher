---
date: 2018-08-13T10:11:21+08:00
tags:
  - app_docker
title: Windows 安装 Docker 并简单使用
slug: windows-docker
share: true
canonicalURL: https://www.jianshu.com/p/d684a507419c
keywords: 
description: 
series: 
lastmod: 
cover:
  image: https://images.unsplash.com/photo-1670121180583-39ab653a071c?q=80&w=720&fm=webp&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
---

初次安装后，首先更换docker镜像地址，打开daemon.json，配置：

- win10配置文件路径，~/docker/daemon.json

```json
{
	"registry-mirrors": ["https://registry.docker-cn.com"]
}
```

或者

右键点击docer运行图标，选择setting >> 右侧Daemon
![24241de0f7506ec2107f130617f1f4a7.webp](/images/24241de0f7506ec2107f130617f1f4a7.webp)


## 1. 登录出错

win10系统
![4215eff2e7da0393b0df7fabfcd5a075.webp](/images/4215eff2e7da0393b0df7fabfcd5a075.webp)

解决办法：打开防火墙

## 2. 查看所有镜像

```
docker image ls
docker images
```

## 3. 查看所有容器

```
docker container ls -all
```

## 4. 镜像与容器的关系

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

## 5. 删除镜像和容器

```
docker rmi <镜像名|镜像ID>
rm <容器名|容器ID>
docker rm `docker ps -aq` # 删除所有容器
```

## 6. 启动与暂停容器

```
docker start <容器名|容器ID>
docker stop <容器名|容器ID>
```

## 7. 查看容器详细信息

```
docker inspect <容器ID|容器名>
```

结果类似于json数据，可以通过| grep来过滤需要的信息

## 8. 运行并进去容器

```
docker run [--name <容器名>] -itd centos bash
```

如果有错误提示，有可能会在命令前加winpty

**补充：**

```
-d # 后台运行
```

## 9. 直接进入容器

```
docker ps # 查看正在运行的容器
docker start <容器名|容器ID> # 如果docker ps 查看没有执行此命令
docker exec -ti <容器名|容器ID> bash
```

如果有错误提示，有可能会在命令前加winpty 

**补充:** 如果不想进入容器执行命令时，可以直接执行`docker exec <容器名|容器ID> <在容器中要执行的命令>` 

## 10. 生成镜像

```
docker commit <容器名|容器ID>
```

## 参考

- [【官网】Docker问题处理](https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization)