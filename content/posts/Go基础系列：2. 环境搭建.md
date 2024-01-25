---  
title: Go基础系列：2. 环境搭建  
date: 2021-07-08T18:18:56+08:00  
share: true  
categories:  
  - Golang  
keywords:  
  - Go  
  - 环境搭建  
description: windows、linux、mac的环境搭建，编辑器goland配置  
series:  
  - Go基础系列  
tags:  
  - program_golang  
slug: golang-install  
cover:  
  image: https://images.unsplash.com/photo-1544257002-830226587caa?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwyMnx8ZW52aXJvbWVudHxlbnwwfDB8fHwxNzAzMzAwOTIzfDA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
我会对 Linux、Windows、Mac 系统下的搭建逐一讲解，包括编辑器的配置。  
  
准备哪些？  
  
1. Go 安装包：[Win Go安装包](https://studygolang.com/dl/golang/go1.16.4.windows-amd64.msi)、[Linux Go安装包（二进制）](https://studygolang.com/dl/golang/go1.16.4.linux-amd64.tar.gz)、[Mac Go安装包](https://studygolang.com/dl/golang/go1.16.4.darwin-amd64.pkg)  
2. Git 环境，Go 下载依赖包借助 Git 工具，这里不做讲解。  
  
## 测试是否安装  
  
版本命令，如果正常输出那就说明已经安装。  
  
```bash  
go version  
  
### 输出  
go version go1.16.4 windows/amd64  
```  
  
## Windows  
  
1. 获取到安装包后，就直接下一步傻瓜式安装，记住自己的安装位置。  
  
![0864f368ac72a7af4f5f6e78e49a4677.png](/images/0864f368ac72a7af4f5f6e78e49a4677.png)  
  
2.  配置环境变量，还有一些不是必须的就不设置了，例如 GOBIN。  
  
![19f6841bcd4ac9118378b0a7ea79ebc7.png](/images/19f6841bcd4ac9118378b0a7ea79ebc7.png)  
  
- GO111MODULE: on，开启 gomod 管理包  
- GOPROXY: [https://goproxy.cn](https://goproxy.cn/)，设置代理，加速依赖包的下载  
- GOPATH: C:\workspace\go （自己定义），工作目录  
- PATH：加入 C:\Program Files\Go\bin 和 C:\workspace\go\bin  
  
3. 运行 Go 的环境命令  
  
```bash  
go env  
  
### 输出，内容省略了一部分，只挑出重点要看的  
set GO111MODULE=on  
...  
set GOPATH=C:\workspace\go  
...  
set GOPROXY=https://goproxy.cn  
...  
```  
  
到这 Windows 的环境就配置完了，简单吧，下来直接往下翻看编辑器的配置。  
  
## Mac  
  
1. 下载好 Mac 环境下的安装包后，直接安装就 OK  
  
![e3bd7126d634479c360ad8974e10c56c.png](/images/e3bd7126d634479c360ad8974e10c56c.png)  
  
2. 环境配置（Windows 那块有讲解含义）  
  
```bash  
### gomod，管理第三方包  
echo 'export GO111MODULE=on' >> ~/.bash_profile  
### 代理，加速依赖包下载  
echo 'export GOPROXY=https://goproxy.cn' >> ~/.bash_profile  
### 工作目录，自定义  
echo 'export GOPATH=/Users/miaogaolin/workspace/go' >> ~/.bash_profile  
### 加入PATH  
echo 'echo PATH=$PATH:$GOPATH/bin' >> ~/.bash_profile  
### 环境变量生效  
source ~/.bash_profile  
```  
  
Mac 的环境安装完了，编辑器安装往下翻。  
  
## Linux  
  
选择我上面提供的安装包是一种 Linux 下通用的方式。那还有其它更简单的方式吗？回答：是有的。  
  
### Ubuntu  
  
```bash  
sudo apt-get golang-go  
```  
  
### Centos  
  
```bash  
sudo yum install go -y  
```  
  
### 推荐  
  
1. 下载安装包  
  
```bash  
wget https://studygolang.com/dl/golang/go1.16.4.linux-amd64.tar.gz  
```  
  
2. 解压  
  
```bash  
tar -xvf go1.16.4.linux-amd64.tar.gz  
```  
  
我解压后的完整目录路径是 /mnt/c/Users/owner/go，可以挪到自己想放置的位置。  
  
3. 配置环境变量  
  
```bash  
### GOROOT,安装路径配置  
echo 'export GOROOT=/mnt/c/Users/owner/go' >> ~/.bash_profile  
### gomod，管理第三方包  
echo 'export GO111MODULE=on' >> ~/.bash_profile  
### 代理，加速依赖包下载  
echo 'export GOPROXY=https://goproxy.cn' >> ~/.bash_profile  
### 工作目录，自定义  
echo 'export GOPATH=/Users/miaogaolin/workspace/go' >> ~/.bash_profile  
### 加入PATH  
echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> ~/.bash_profile  
### 环境变量生效  
source ~/.bash_profile  
```  
  
## 编辑器  
  
这里我推荐使用 Goland，下载链接：[https://www.jetbrains.com/go/download](https://www.jetbrains.com/go/download/#section=windows)。  
  
安装过程就不描述了，直接从配置开始。如果想要破解的小伙伴，可以悄悄问我。  
  
1. 打开项目目录， File > Settings > Project Structure，如果没有设置，点击 +Add Content Root 进行设置。  
  
![b9854bf8f66a0c3ee9efe1edb6ee01fd.png](/images/b9854bf8f66a0c3ee9efe1edb6ee01fd.png)  
  
2. GOROOT 的检查，File > Settings > Go > GOROOT，如果没有像如图所示。点击选择 Local，加载你安装的 Go 目录。  
  
![4352a86be2440f770af3a5abbe74774b.png](/images/4352a86be2440f770af3a5abbe74774b.png)  
3. GOPATH 的 i 检查， File > Settings > Go > GOPATH，如果在配置环境变量时没有问题，则自动显示 GOPATH 的路径。  
  
![ba961c23d92cd1f979bff252be85c184.png](/images/ba961c23d92cd1f979bff252be85c184.png)  
  
4. 配置两个工具  
  
- goimports，写代码时自动导入第三方包，需要下载，运行以下命令后会在 $GOPATH/bin，生成一个二进制文件。  
  
```bash  
go get [golang.org/x/tools/cmd/goimports](http://golang.org/x/tools/cmd/goimports)  
```  
  
- gofmt，自动格式化，自带的无需下载  
  
下来在 Goland 配置这两个工具，打开 File > Settings > Tools > File Watchers，点击如图的加号导入。  
  
![e434710df2a85f69a7689aed4ae3bb4b.png](/images/e434710df2a85f69a7689aed4ae3bb4b.png)  
  
5. 配置完毕，来一个 "Hello World" 代码体验下。先看看目录如何建。  
  
![cdb42d559d68b182c9893f9036647b79.png](/images/cdb42d559d68b182c9893f9036647b79.png)  
  
- bin: 自动生成，当运行上面的 go get 命令时，自动生成了个 bin 目录，放置可运行的二进制文件。  
- pkg: 自动生成，产生的第三方包会自动放置在该目录下。  
- src: 手动新建的目录，gobasic 是我的写代码的目录，你也可以在 src 下新建其它目录。  
  
hellworld.go 文件代码如下：  
  
```go  
// 入口文件的包名  
package main  
  
// 引用包  
import "fmt"  
  
// 入口函数  
func main() {  
	 // 控制台输出  
   fmt.Println("Hello World!")  
}  
```  
  
控制台运行如下命令：  
  
```bash  
### 初始化 gomod   
go mod init  
### 运行  
go run hellworld.go  
  
### 输出  
Hello World!  
```  
  
## 总结  
  
本篇讲解了 Go 环境的搭建，和编辑器的配置。在实际搭建时，每个人可能遇到不同的问题，一旦遇到不知道怎么处理的，请在下方评论！