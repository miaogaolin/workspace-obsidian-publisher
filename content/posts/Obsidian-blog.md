---
date: 2023-12-21T10:06:19+08:00
tags:
  - obsidian
  - github
  - hugo
  - vercel
title: 手递手教你 Obsidian 笔记建站
slug: obsidian-blog
share: true
canonicalURL: 
keywords:
  - obsidian
  - blog
  - github publisher
  - paperMod
  - hugo
  - vercel
  - Github
description: 
series: 
lastmod: 2023-12-27T16:32:00
cover:
  image: /images/Obsidian-blog-20240113002109537.webp
author: hellloveyy & 老苗
---



### 5. 发布新文章 

当然插件的配置也支持菜单模式，配置前往：`Plugin settings - Menu`。  
先使用命令行 `command + P` ，输入 ` active `，然后选择 `Github Publisher: Uxxxxxxx` 即可，记着文章的 share 属性要开启，即 true。  
![](/images/Obsidian-blog-20240114012431514.webp)
  
右下角会提示上传的进度，如果完成了右上角有提示：  
![](/images/Obsidian-blog-20240114013047211.webp)
如果你想上传多个 share 为 true 的文章，使用的命令为：  
- `Refresh published and upload new notes` 将所有 share 为 true 且新更新的文章发布  
- `Refresh all published notes` 将所有 share 为 true 的文章都发布  

### 6. Logo & favicon 设计
推荐两个工具：
- [腾讯推出的AI设计Logo工具](https://ailogo.qq.com/guide/brandname)  ![](/images/post-20240111195047056.webp)
- [标小智](https://www.logosc.cn/logo/favicon) 这里下载过后直接解压拖动所有文件全部覆盖即可！![](/images/Obsidian-blog-20240114011233104.webp)

### 7. Vercel 部署  
接下来访问 [vercel](https://vercel.com/) 官网，然后将上面对应的仓库部署上去即可。  
##### 1. Github  
  
使用 Github 登录，因为要读取你 GitHub 里面的项目。

在部署之前记得把所有当前的改动都保存推上去哦。

```
git add -A
git commit -a -m 'first'
git push origin main
```

##### 2. 创建项目  
![](/images/Obsidian-blog-20240114013913729.webp)
  
  
##### 3. Import & Deploy  
  
导入 Github 上 fork 后的项目，**import** 后再点击 **deploy**，注意要选择环境 `hugo` 接下来需要耐心等撒花即可。  
![](/images/Obsidian-blog-20240114014039357.webp)
![](/images/Obsidian-blog-20240114014119226.webp)
  
##### 4. 域名  
  
完成后点击 Domains，添加自己的域名。  
![](/images/Obsidian-blog-20240114014309645.webp)
![](/images/Obsidian-blog-20240114014347570.webp)
添加完后，再解析自己的域名，我的域名在 [dynadot.com](/images/dynadot.com) 购买的，按照说明设置即可。Vercel 站上给你分配的域名，网络不太稳定国内访问有时候会受限。  
![](Obsidian-blog-20240114014649113.webp)

## 四、总结 

生命在于折腾！欢迎以任何方式咨询任何问题，能不能回答到时候再说