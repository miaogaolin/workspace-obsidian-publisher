---  
title: 使用了WSL2后，Docker 太慢，如何解决？  
date: 2021-07-23T10:25:45+08:00  
share: true  
categories:  
  - docker  
keywords:  
  - WSL2  
description: docker开启了WSL2，运行时太慢、Vscode 配置 WSL  
slug: docker-wsl2  
cover:  
  image: https://images.unsplash.com/photo-1595236754046-2da8684b47da?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwxMDZ8fHNwZWVkfGVufDB8MHx8fDE3MDI5NjczNjR8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
  
最近在 Win10 系统下使用 Docker 时发现有点慢，经过一番查访后，原来原因是 Docker 开启了 `WSL2` 的锅。对于好奇心这么强的我，关掉 WSL2 不是我能做出来，毕竟 WSL2 是新东西，要研究下具体怎么用。  
  
## 环境  
  
1. Win10 系统  
2. 安装了 Ubuntu 子系统  
  
## 是否开启 WSL2  
打开 Docker 的设置, 查看截图中红色框的复选框是否选中，如果选中说明启用了 `WSL2`。  
![20231208111292.png](/images/20231208111292.png)  
  
## 什么是 WSL2  
既然遇到了 `WSL2` 新概念，那它是什么？有什么优点？下来粗略的了解下。  
  
> 官话是这么说的，“WSL 2 使用最新、最强大的虚拟化技术在轻量级实用工具虚拟机 (VM) 中运行 Linux 内核。” [官网传送门](https://docs.microsoft.com/zh-cn/windows/wsl/compare-versions)  
  
我一句话概括，**“在 Win10 系统下给装了个虚拟机，可以安装 Linux 系统”**。  
  
### 1. 为什么慢  
因为，你在 Win10 下的文件，在 Linux 环境下运行，**文件进行跨系统了**。  
  
所以，为了更快的性能，只要将文件移入 Linux 系统中，不出意外应该就快了。  
  
### 2. Win10 文件移入 Linux  
  
当你 Win10 系统下，安装了 Ubuntu 后，可以在文件管理访问如下目录：  
```shell  
\\wsl$\Ubuntu  
```  
![20231208111275no1.webp](/images/20231208111275no1.webp)  
  
这个就是安装好的 Ubuntu 系统，下来只要将文件拖入你的家目录，我的家目录是 `home/miaogaolin`。拖入其它地方，是没有权限的（可能会有提高权限的方法，我没有研究）。  
  
## Ubuntu 中使用 Docker  
在 Win10 系统下安装好了 Docker，在 Ubuntu 下就不需要安装了，只需要按照如下截图设置下，就可以在 Ubuntu 下使用 Docker 命令。  
![20231208111297.webp](/images/20231208111297.webp)  
  
注：设置后，重启。  
  
  
进入 Ubuntu 试试 Docker 命令。  
  
![20231208111225.png](/images/20231208111225.png)  
  
按照如图，运行 `docker ps` 命令，如果没有报错，那说明没有问题。下来你就可以在这个新地盘启动自己所有的服务了。  
  
那你有没有想过这么一问题，我换了新地盘，怎么直接在 Ubuntu 系统中开发我的项目，继续往下看。  
  
## Vscode 和 WSL2  
  
这块我是用了 Vscode 编辑器写我的项目代码，如果是其它的编辑器的话，直接在此目录下  
`\\wsl$\Ubuntu` 找到自己的项目并打开（具体我没有测试，不知道会遇到什么问题）。  
  
在 Vscode 编辑器中安装“连接 WSL2 的扩展”，这样直接可以在编辑器操作 Ubuntu 下的文件。  
  
![20231208111253.png](/images/20231208111253.png)  
安装完后重启，重启后按照如下图点击左下角，选择自己要打开的目录, 选择好等一会就好。了。  
![20231208111247.webp](/images/20231208111247.webp)  
  
  
下来看看我的弄好后是什么样子，你看着我的截图红色框对比下，自己是不是也好了。如果喜欢在 Linux 下工作，这也是个好办法。  
  
![20231208111223no1.webp](/images/20231208111223no1.webp)  
  
## 总结  
  
本篇讲解了 WSL2 对速度的影响，并且提供了一个在 Win10 系统下在 Linux 环境下开发的方法。  
  
如果遇到不懂的，就在下方留言，不要客气！  
