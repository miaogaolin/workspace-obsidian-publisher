---  
date: 2022-02-27T13:44:53+08:00  
tags:   
title: Golang在线工具迁入Vercel  
slug: tools-vercel  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
lastmod:   
cover:  
  image: https://images.unsplash.com/photo-1600493572882-f098876ce680?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwyMXx8dG9vbHxlbnwwfDB8fHwxNzAyOTY1MTMxfDA&ixlib=rb-4.0.3&q=80&w=720  
author:   
---  

  

  

  
> 2023/12/19 由于网站主题更换，为了使在线工具切换主题也能访问，就将 [https://printlove.cn/tools/](https://printlove.cn/tools/) 路径做了跳转。
  

  
不知道还有朋友在使用 [https://printlove.cn/tools/](https://printlove.cn/tools/) 这几个工具没，因为访问人数不多，我就给将网站停用了，真实原因是还要花钱续服务器，所以就...
  

  
在停用的几天，有 1 个朋友说挺好用的，但网站停了。为了不让这位朋友寒心，网站就重新启用了。
  

  
这次不再购买服务器，选择了 Vercel 平台管理我的网站，原因如下：
  

  
- 支撑 hugo 工具
  
- 支持部署 Go 代码，还支持 Node.js、Python、Ruby
  
- 免费
  

  
如果你对免费部署一个网站感兴趣，可以看看我的这篇 [Vercel + Notion 建个人博客]({{< relref "Vercel%20+%20Notion%20%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}})。
  

  
# Hugo 部署
  

  
[printlove.cn](http://printlove.cn) 网站内容是通过 Hugo 工具生成的，所以先解决此问题。
  

  
步骤如下：
  

  
1. 使用 Github 登录 [Vercel](https://vercel.com/) 平台。
  
2. 导入 [printlove](https://github.com/miaogaolin/printlove) 仓库，如果是本地代码部署，待会会讲。
  
3. 配置部署
  

  
![503ba067ef76b481bfc2c5bb9d59d3bb.webp](/images/503ba067ef76b481bfc2c5bb9d59d3bb.webp)
  

  
如果在开始部署时，出现了报错，那就需要指定 hugo 的版本。我的项目是遇到了，原因是 vercel 默认的版本存在 Bug。
  

  
通过设置环境变量指定版本：
  

  
![c8b3f8930b0e406e90e3fc32c33d36f4.webp](/images/c8b3f8930b0e406e90e3fc32c33d36f4.webp)
  

  
# Go 代码部署
  

  
完成了静态网页的部署，下来就要搞 API 接口代码。
  

  
其中除了 json 转 Go Struct 工具外，其它的工具都是使用 Go 代码实现，开始整。
  

  
1. 在项目根目录下创建 api 目录。
  
2. 在目录下创建 tool.go 文件，这个文件名称随意，内容如下：
  

  
```go
  
package api
  

  
import (
  
	"net/http"
  
)
  

  
func Tool(w http.ResponseWriter, r *http.Request)  {
  
	// todo
  
}
  
```
  

  
vercel 平台会将后面的请求传递给该 Tool 函数，这个函数名也随意，但函数参数必须按照该规则。
  

  
3. 在项目根目录创建 vercel.json 配置文件，负责路由，内容如下：
  

  
```go
  
{
  
  "routes": [
  
    {
  
      "src": "/api/tool",
  
      "dest": "/api/tool.go"
  
    }
  
  ]
  
}
  
```
  

  
- src：接口地址
  
- dest：将请求转交给 `tool.go` 文件处理。
  
4. 重新部署即可。
  

  
**注意坑**：
  

  
第一次我在部署 Go 代码时，在 api 目录下创建了好多个 Go 文件，每一个文件负责一个接口处理，但在部署时，出现失败。
  

  
**原因**：多个文件编译时占用的内存太大，超过了 vercel 提供的免费内存大小（1024M）。因此，我将所有的接口处理都集成在了 tool.go 文件内，详细的请看 [源码](https://github.com/miaogaolin/printlove/blob/master/api/tool.go)。
  

  
# 本地部署
  

  
有时候我们想测试本地的代码在 Vercel 平台的部署结果，那提交到 Github 就有问题：“麻烦和不合理”。
  

  
下来使用命令进行部署。
  

  
## 1. 安装
  

  
```bash
  
npm i -g vercel
  
```
  

  
## 2. 登录 vercel
  

  
```bash
  
vc login
  
```
  

  
vc 是 vercel 的缩写命令。
  

  
## 3. 开始部署
  

  
下来在项目根目录运行 `vc` 命令，执行后 vercel 网站就会出现本地项目部署的接口。
  

  
![abd30703b0988b28ff04c43f0acb2c7a.webp](/images/abd30703b0988b28ff04c43f0acb2c7a.webp)
  

  
如图 printlove-test 就是我本地代码的测试项目。
  

  
# 总结
  

  
这篇文章我没有过多深究 vercel 的太多细节，只是把我使用到的，觉得够用的东西拿出来分享给大家。
  

  
迁移成果：
  

  
- 网站：[printlove.cn](https://printlove.cn)
  
- 源码：[github.com/miaogaolin/printlove](https://github.com/miaogaolin/printlove)