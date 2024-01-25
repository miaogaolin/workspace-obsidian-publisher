---  
date: 2021-12-19T21:20:31+08:00  
tags:   
title: Vercel 解决静态网站接口跨域  
slug: vercel-cross  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
lastmod:   
cover:  
  image: https://images.unsplash.com/photo-1549810029-947c46bc3a5c?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwyNHx8c3RhdGljfGVufDB8MHx8fDE3MDMzMzgzMjR8MA&ixlib=rb-4.0.3&q=80&w=720  
author:   
---  
  
当你的站是一个静态网站时，并且网站中有接口请求时，可能会出现跨域问题。那解决该问题的大部分的办法就是使用一个服务器做代理转发。  
  
所以说到这就知道，还需要一个服务器，即：要花钱。那有什么办法吗？  
  
说出来了当然有。  
  
##  前提  
  
使用 vercel 托管你的静态网站。  
  
## 开始配置  
  
###  1. vercel.json  
  
在你的项目根目录创建一个 vercel.json 文件，内容如下：  
  
```json  
{  
    "rewrites": [  
        {  
            "source": "/notion-api/(.*)",  
            "destination": "/api/proxy"  
        }  
    ]  
}  
```  
  
•source：不直接请求接口地址，而是按照 source 格式的方式访问，该格式自己定义。•destination：将 source 匹配到的 url 转发到 destination 地址上。  
  
### 2. destination  
  
这个地址是在 vercel 上启动一个常驻服务生成的。  
  
在项目根目录创建一个 api/proxy.js 路径的文件，按照这个路径要求来，该文件的内容如下：  
  
```js  
// req：请求  
// res：响应  
module.exports = (req, res) => {  
    // ...  
}  
```  
  
然后在这个函数体内可以自己编写请求的转发。  
  
看看我的写过的一个例子，用来转发 Notion API 接口。  
  
完整例子：  
  
```js  
const request = require('request');  
  
module.exports = (req, res) => {  
  // proxy middleware options  
  let prefix = "/notion-api"  
  if (!req.url.startsWith(prefix)) {  
    return;  
  }  
  let target = "https://api.notion.com" + req.url.substring(prefix.length);  
  
  var options = {  
    'method': 'GET',  
    'url': target,  
    'headers': {  
      'Notion-Version': res.headers['notion-version'],  
      'Authorization': res.headers['authorization']  
    }  
  };  
  request(options, function (error, response) {  
    if (error) throw new Error(error);  
    res.writeHead(200, {"Content-Type": "application/json"});  
    res.write(response.body);  
    res.end();  
  });  
}  
```  
  
该例子，就是从 req 参数里获取想要的数据，然后使用 request 发放进行转发。  
  
### 2. Functions  
  
写好后，如何运行这个文件呢？  
  
在 Vercel 后台找到 Functions 这个功能板块，如果上面的配置没有问题，并且已经重新部署，那么就会出现如下图：  
  
![](/images/096396f727dafb34f78d86af313214b4.webp)  
  
配置好后，就可以测试了。  
  
假如我的静态网站在 Vercel 上的地址是 https://laomiao.site，那么根据 version.json 文件中的配置访问：  
  
https://laomiao.site/notion-api/xxxxxxx  
  
成功后会在 Functions 页面出现访问日志，如果没有那就说明有问题。  
  
## 总结  
  
我捋下流程。  
  
![](/images/ac23066af80b695b9dab31a045d93c2f.webp)  
