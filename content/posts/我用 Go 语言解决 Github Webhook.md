---  
title: 我用 Go 语言解决 Github Webhook  
date: 2021-11-01T17:51:00+08:00  
share: true  
categories:  
  - Golang  
keywords:  
  - Github  
  - Webhook  
description: 使用 webhook 工具快速完成 Github 钩子的配置  
slug: go-webhook  
cover:  
  image: https://plus.unsplash.com/premium_photo-1676511249826-2adf52caabee?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
---  

  

  
Github  Webhook(钩子) 说简单点，就是当你的项目触发了某个动作时，例如 `git push` 时，可以通知给一个线上可访问的地址。
  

  
这块我接收通知用来自动更新我服务器上的网站，不用再自己登录服务器更新。
  

  
首先要安装 Go 环境，查看 [《环境搭建》]({{< relref "Go%E5%9F%BA%E7%A1%80%E7%B3%BB%E5%88%97%EF%BC%9A2.%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.md" >}})。
  

  
## 添加 webhook
  

  
打开自己的 github 项目，然后按照如图所示，打开 Settings > Webhooks。
  

  
![20231208111274.webp](/images/20231208111274.webp)
  

  
打开后配置三项数据：
  

  
1. Payload URL：自己服务的 API 接口地址，用来接受 github 的通知。
  
2. Content type：选择 application/json，表示使用 json 数据通知。
  
3. Secret：密钥，用来防止 API 接口地址被他人访问。
  

  
## 安装
  

  
因为要在自己服务器上写一个 API 接口，并且要处理密钥，所以我自己写了一个工具，根据配置文件就可以自定义启动服务。
  

  
先安装。
  

  
```bash
  
go get github.com/miaogaolin/webhook
  
```
  

  
## 配置
  

  
在自己的项目下创建一个配置文件 hooks.json。
  

  
```bash
  
{
  
  "bind": ":9000",
  
  "items": [
  
    {
  
      "repo": "https://github.com/miaogaolin/printlove",
  
      "branch": "main",
  
      "script": "deploy.sh",
  
      "secret": "123"
  
    }
  
  ]
  
}
  
```
  

  
- bind：端口号
  
- repo：接受的 github 地址。
  
- branch：接受的分支。
  
- script：需要执行的脚本，这个一会咱自己写。
  
- secret：上面截图中填写的密钥。
  

  
下来在自己项目下写一个 deploy.sh 脚本，用来自动更新网站的代码，这块你根据自己的实际情况写。
  

  
```bash
  
#!/bin/bash
  
git pull
  
hugo -D
  
```
  

  
因为我的网站是用 hugo 工具生成的，所以才有了 `hugo -D` 命令。
  

  
## 启动
  

  
一切准备好后，就可以启动 webhook 工具。
  

  
```bash
  
webhook hooks.json
  
```
  

  
如果选择后台跑，使用下面命令。
  

  
```bash
  
nohup webhook hooks.json &
  
```
  

  
运行该命令会在当前目录下自动生成 nohup.out 文件，该文件存储 webhook 工具的日志。
  

  
## 配置 Nginx
  

  
如果你不想在使用 "IP+ 端口 " 形式来接受 Github 通知时，那配置个 Nginx 转发。
  

  
```bash
  
server {
  
		
  
		server_name .printlove.cn
  
		...
  

  
    location /hook {
  
        rewrite ^/hook / break;
  
        proxy_pass http://127.0.0.1:9000;
  
    }
  
}
  
```
  

  
该配置文件用来将 https://www.printlove.cn/hook 的请求转发到 9000 端口上。
  

  
配置好后，重启。
  

  
```bash
  
nginx -s reload
  
```
  

  
## 验证成功
  

  
所有的都配置完成后，就可以给自己的项目中 `git push` ，再查看 webhook 工具的运行日志，成功会出现如下日志：
  

  
```bash
  
Run ./deploy.sh output: Updating 58ffcfd..81918f1
  
```
  

  
一切完成。