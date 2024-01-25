---  
title: Go基础系列：3. 环境搭建疑惑 - gomod学习  
date: 2021-07-09T18:18:56+08:00  
share: true  
categories:  
  - Golang  
keywords:  
  - go  
  - gomod管理  
description: gomod如何使用，不同配置的含义，gomod命令的作用  
series:  
  - Go基础系列  
slug: gomod  
cover:  
  image: https://images.unsplash.com/photo-1588665343610-04dfd562e2e7?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwzOHx8cXVlc3Rpb258ZW58MHwwfHx8MTcwMzMwMTczNXww&ixlib=rb-4.0.3&q=80&w=720  
---  
  

  
本篇是对大家在搭建环境时遇到的 gomod 问题产生的疑惑进行讲解。
  

  
## 什么是 gomod
  

  
全名 go modules，简单说就是 go 项目用来管理第三方包的工具。此特性是 golang 1.11 版本开始增加的。
  

  
## 三个设置
  

  
在”Go 基础系列 | 环境搭建“那篇文章中，讲解了设置 GO111MODULE 环境变量为 on，不懂了可以看看那篇文章，那里会有针对不同系统的讲解。但 GO111MODULE 环境变量其实有三个值可以设置，具体如下：
  

  
- on：强制启用 gomod 管理第三方包，因此项目目录下必须包含 go.mod 文件。
  
- off：通过当前项目下 vendor 目录或者 GOPATH 模式来查找。
  
- auto：如果当前项目下有 go.mod 文件，就采用 on 的形式，不存在采用 off 的形式。
  

  
注：实际开发中建议设置为 on
  

  
## 常见命令
  

  
### 1. 初始化
  

  
初始化命令，会在当前目录下生成 go.mod 文件，在一个项目中，此文件只需要一份，放置在项目根目录。
  

  
```bash
  
go mod init 
  
## 自定义 module 名称
  
go mod init study
  
```
  

  
go.mod 内容如下：
  

  
```bash
  
## 当前目录是study，自动生成
  
module study
  

  
go 1.16
  
```
  

  
### 2. 自动更新
  

  
自动下载当前项目所依赖的第三方包，会自动生成 go.sum 文件，该文件包含当前项目所依赖的第三方包所依赖的所有具体版本。如果项目中删除了某个依赖包，也会从 go.mod 文件中删除。
  

  
```bash
  
go mod tidy
  
```
  

  
### 3. 拷贝 vendor
  

  
自动将依赖的第三方包拷贝到 vendor 目录下。
  

  
```bash
  
go mod vendor
  
```
  

  
### 4. 其它
  

  
以上是我经常使用到的命令，剩下的命令很少使用。
  

  
- go mod download：下载依赖包，当前项目中如果没有引入是不会进行下载，下面会说个实际使用的操作
  
- go mod edit：编辑 go.mod
  
- go mod graph：打印模块依赖图
  
- go mod verify：验证依赖是否正确
  
- go mod why：解释为什么需要依赖
  

  
## Goland 引入包
  

  
说说我在实际开发中的一个流程，假如现在你也使用 Goland 的编辑器进行写代码，并且开启了 gomod，那如何引入一个新的包，流程如下。
  

  
### 1. 下载
  

  
在自己项目下，使用 go get 命令下载包，假如下载 gin 框架。
  

  
```bash
  
## 拉取最新的
  
go get github.com/gin-gonic/gin
  
```
  

  
运行完成后，会在 go.mod 文件内引入此包，文件内容如下：
  

  
```bash
  
module study
  

  
go 1.16
  

  
require github.com/gin-gonic/gin v1.7.2 // indirect
  
```
  

  
也可以下载指定版本
  

  
```bash
  
## 特定版本，后面跟的是 git 中设置的tag
  
go get github.com/gin-gonic/gin@v1.7.1
  

  
## 拉取具体的某个commit，commit id可略写
  
go get  github.com/gin-gonic/gin@34ce2104cad324f444943c528746bf6d23643cd3
  
```
  

  
### 2. 开启 gomod 包提示
  

  
左上角点击 File，Setting > Go > Go modules ，开启 Enable Go modules integration。开启后在调用包时会有提示，并且可以通过 ctrl + 鼠标左键，查看源码。
  

  
![0d1907b0af27de66e89b979e48c3ae12.png](/images/0d1907b0af27de66e89b979e48c3ae12.png)
  

  
### 3. 自动引入
  

  
前提是引入了 goimport 工具，在 ”Go 基础系列 | 环境搭建“一篇中有讲解如何配置，下来开始写代码了。
  

  
![ff5b30392401a5619af9164726ce2603.png](/images/ff5b30392401a5619af9164726ce2603.png)
  

  
手写一部分代码时，会自动提示，回车后会自动引入 [github.com/gin-gonic/gin](http://github.com/gin-gonic/gin) 包，完整代码如下：
  

  
```go
  
package main
  

  
import "github.com/gin-gonic/gin"
  

  
func main() {
  
	gin.New()
  
}
  
```
  

  
## 进一步
  

  
掌握了以上知识，应该不会在入门阶段被卡在门外了，现在对于 go.mod 文件内容进行整体的认识。
  

  
```go
  
// 项目名，别人引入你这个项目的路径
  
module github.com/a/b
  

  
// 当前环境的版本号
  
go 1.16
  

  
// 依赖
  
// 出现 indirect 表示项目中还未使用
  
require (
  
        one/thing v1.3.2
  
        other/thing v2.5.0 // indirect
  
        ...
  
)
  

  
// 排除具体的版本
  
// 如果某个版本出现重要bug,可以这么做
  
exclude (
  
        bad/thing v0.7.3
  
)
  

  
// 替代
  
replace (
  
// 项目中通过src/thing引入，但实际引入的位置是dst/thing，相当于取个别名
  
        src/thing 1.0.2 => dst/thing v1.1.0
  
)
  
```
  

  
### 最小版本选择
  

  
如果当前项目和依赖的第三方包中出现了引入同一个包时，但不是同一版本，这时候会选择一个最小版本引入。
  

  
## 总结
  

  
对于以上知识，我觉得在实际开发中够用了，如果你遇到了我文中没有提及到的问题，但不知道如何解决，就在下方留言！