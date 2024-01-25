---  
date: 2023-12-21T10:06:19+08:00  
tags:  
  - app_obsidian  
title: 使用 Obsidian 免费建个人博客  
slug: obsidian-blog  
share: true  
canonicalURL:   
keywords:  
  - obsidian  
  - blog  
  - github publisher  
  - hugo  
  - vercel  
  - Github  
description:   
series:   
lastmod: 2023-12-27T16:32:00  
cover:  
  image: https://images.unsplash.com/photo-1682687220211-c471118c9e92?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
author:   
---  
  
目前我的网站发布流程就是使用该篇文章技术，如果你使用的 Notion 写文章，可以看看这篇 [Vercel + Notion 建个人博客]({{< relref "Vercel%20+%20Notion%20%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}}) 。  
  
流程简介：  
1. 选择自己的静态网站生成工具，我选择 Hugo；  
2. 使用 Hugo 初始化好结构然后上传到 Github 仓库；  
3. Obsidian 写好文章，然后使用插件将指定的文章 (md 格式) 上传到第 2 步的仓库,插件的作用就是将 Obsidian 中的 markdown 内容适配成 Hugo 需要的内容；  
4. 部署仓库，配置域名。  
## 相关工具  
1. Obsidian + Github Publisher 插件，必须  
2. Hugo + Paper Mod 主题，可选择其它  
3. Github，必须  
4. Vercel，可选择其它  
  
具体步骤继续往下看。  
## Hugo + PaperMod  
使用 hugo 初始化一个网站，并配置好你喜欢的主题，并发布到 Github 上，这块具体怎么弄就不展开介绍了。  
  
可以参考：  
- 官方主题文档：[PaperMod](https://adityatelange.github.io/hugo-PaperMod/)  
- 我的仓库：[miaogaolin/workspace-obsidian-publisher](https://github.com/miaogaolin/workspace-obsidian-publisher) 稍微改了点官方主题  
  
## Github Publisher  
给 Obsidian 安装 Github Publisher 插件，该插件的作用是将 Obsidian 中的文章和本地附件上传到 Github 仓库，上传前可以指定文件目录、自定义内容替换等操作。  
  
  
### Github config  
![2fd44d1493f8c3f6eb2a342853810937.png](/images/2fd44d1493f8c3f6eb2a342853810937.png)   
  
注意：  
1. 生成的 token 不要放在 Github 的公共仓库，检测到 token 就会失效。  
2. 通过 here 生成 token 时的 [链接](https://github.com/settings/tokens/new?scopes=repo,workflow) 会自动带上权限，你只需要设置名字和过期时间即可。  
  
### 默认配置  
  
根据我的使用情况，我保存了一份设置，你可以直接使用，如果想了解为何这样，可以看看： [Github Publisher 插件适配 Hugo 的配置]({{< relref "Github%20Publisher%20%E6%8F%92%E4%BB%B6%E9%80%82%E9%85%8D%20Hugo%20%E7%9A%84%E9%85%8D%E7%BD%AE.md" >}})。  
  
前往 [miaogaolin/obsidian-github-publisher-hugo](https://github.com/miaogaolin/obsidian-github-publisher-hugo) 拷贝 settings.json 设置，然后粘贴导入插件：  
![e391eb4a6c68184f665340c112cffe98.webp](/images/e391eb4a6c68184f665340c112cffe98.webp)  
  
  
## Obsidian 文章模板  
我完整说说我在 obsidian 模板里配置的内容，用于发布文章时统一的设置。  
我的配置是和 Hugo 强关联的，如果你用了其它工具，就根据自己的情况调整。  
```yaml  
---  
date: "{{date}}" # 创建时间，我这边生成的格式是 YYYY-MM-DDTHH:mm:ssZ  
tags:   
	- 标签1  
	- 标签2  
title: "{{title}}"  
slug: "{{time}}" # 自定义 URL 中文章的访问名称，默认用时间戳填充模板格式为X  
share: false  # 配合 Github Publisher插件用的,true表示 obsidian 的文章可以发布  
canonicalURL: "" # 之前文章在其他地方被发布的地址，避免搜索引擎重复，设置了该属性会优先展示 canonicalURL 执行的文章  
keywords:   # 用于 SEO 优化，也可以不配置该内容默认会使用 tags 的内容  
	- 关键字1  
	- 关键字2  
description: "" # 文章的描述 SEO 优化，为空时默认会截取文章前面的内容  
series: "系列" # 系列文章  
lastmod:  # 文章最后更新的时间  
lang: "cn" # 默认不用写，配置文件会设置默认 cn 中文，en 英文等等  
cover.image: "" # 文章封面图片地址  
author: # 作者名称  
dir: # 搭配 Github Publisher 插件设置文章上传的目录  
---  
```  
- dir 属性：不用设置这个默认会上传到 content/posts 目录下，如果设置了会以 content 为根目录，例如：[关于我]({{< relref "%E5%85%B3%E4%BA%8E%E6%88%91.md" >}})、[赞助]({{< relref "%E8%B5%9E%E5%8A%A9.md" >}}) 这两篇文章，我设置的就是 `dir: ./`  
- `cover.image`：设置封面，在使用 Github Publisher 后会转化为二级 key。  
## 发布  
  
### Obsidian 命令  
当然插件的配置也支持菜单模式，配置前往：Plugin settings -> Menu。  
  
先使用命令行发布，输入 active，然后选择 Github Publisher 即可，记着文章的 share 属性要开启，即 true。  
![01bd34047086ccab150386a9fbee2e6a.webp](/images/01bd34047086ccab150386a9fbee2e6a.webp)  
  
右下角会提示上传的进度，如果完成了右上角有提示：  
![2f4eaef491242a63a5099537f5ff8a5f.webp](/images/2f4eaef491242a63a5099537f5ff8a5f.webp)  
如果你想上传多个 share 为 true 的文章，使用的命令为：  
- Refresh published and upload new notes 将所有 share 为 true 且新更新的文章发布  
- Refresh all published notes 将所有 share 为 true 的文章都发布  
### Vercel 部署  
接下来访问 [vercel](https://vercel.com/) 官网，然后将上面对应的仓库部署上去即可。  
> 你也可以选择 Github Pages、Netlify 部署，甚至自己的服务器也行。  
#### 1. Github  
  
使用 Github 登录。  
  
#### 2. 创建项目  
  
![820895b5d2f3b4a8f0cfcbe729713b1b.webp](/images/820895b5d2f3b4a8f0cfcbe729713b1b.webp)  
  
#### 3. Import & Deploy  
  
导入 Github 上 fork 后的项目，**import** 后再点击 **deploy**，下来需要耐心等会。  
  
![f82042806c13baba0ff30bf60510df39.webp](/images/f82042806c13baba0ff30bf60510df39.webp)  
  
![7cda90de3afb93df2543fe2009f6e6a7.webp](/images/7cda90de3afb93df2543fe2009f6e6a7.webp)  
  
  
#### 4. 域名  
下来输入自己的域名，然后 ADD，选择推荐的转发规则即可，按照给的提示信息去解析自己的域名。  
  
![c370b2dc6029ed604493b2ec1e1e6e9a.webp](/images/c370b2dc6029ed604493b2ec1e1e6e9a.webp)  
![e6aee6766b6bfe93a06cae9c8016ac65.webp](/images/e6aee6766b6bfe93a06cae9c8016ac65.webp)  
## 优化内容的相关插件  
  
这些 Obsidian 插件对于发布网站不是必要的，但是对于优化内容格式还是很有必要的：  
- Obsidian Linter 插件,我只用了在英文两边加空格的设置。  
- Image Converter 转化图片格式，我统一转为 webp，并设置了图片分辨率大小。  
- Unique attachments 用于将附件的文件名统一为 “字母 + 数字”的格式,记着在配置里加入 webp 图片格式  
- Image Inserter 用于找图片，我用于设置文章封面，即设置 `cover.image` 属性。  
## 总结  
重点学会了 Github Publisher 插件如何将 Obsidian 中的文章格式适配成你最终想要的内容格式即可。  
  
至于你选择 Hugo 静态网站生成器，还是 Hexo 都是可以的。   
