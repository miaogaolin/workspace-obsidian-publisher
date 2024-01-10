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
  image: https://images.unsplash.com/photo-1682687220211-c471118c9e92?q=80&w=500&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
author:   
---  
  
目前我的网站发布流程就是使用该篇文章技术，如果你使用的 Notion 写文章，可以看看这篇 [Vercel + Notion 建个人博客]({{< relref "Vercel%20+%20Notion%20%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.md" >}}) 。  
## 相关工具  
1. Obsidian + Github Publisher 插件  
2. Hugo + Paper Mod 主题，你也可以选择其它，例如：Hexo  
3. Github  
4. Vercel  
  
具体步骤继续往下看。  
## Hugo + PaperMod  
使用 hugo 初始化一个网站，并配置好你喜欢的主题，并发布到 Github 上，这块具体怎么弄就不展开介绍了。  
  
可以参考：  
- 官方主题文档：[PaperMod](https://adityatelange.github.io/hugo-PaperMod/)  
- 我的仓库：[miaogaolin/workspace-obsidian-publisher](https://github.com/miaogaolin/workspace-obsidian-publisher) 稍微改了点官方主题  
  
## Github Publisher  
给 Obsidian 安装 Github Publisher 插件，该插件的作用是将 Obsidian 中的文章上传到 Github 仓库，上传前可以指定文件目录、自定义内容替换等操作。  
  
我会根据使用过程，讲一讲我用到的配置，如果你对其它配置感兴趣，可以看看 [官网文档](https://obsidian-publisher.netlify.app/plugin/)。  
### Github config  
![2fd44d1493f8c3f6eb2a342853810937.png](/images/2fd44d1493f8c3f6eb2a342853810937.png)   
  
注意：生成的 token 不要放在 Github 的公共仓库，检测到 token 就会失效。  
### Upload config  
![53917e2e2c04940aac42ceb567ccc769.webp](/images/53917e2e2c04940aac42ceb567ccc769.webp)  
Fixed Folder，表示将所有的文章上传到 content/posts 目录下。    
![01ee5739a16e4235cdae2095a45aa5e3.webp](/images/01ee5739a16e4235cdae2095a45aa5e3.webp)  
Property key，可以通过文章的属性设置上传的目录，例如我这配置：    
- 通过 dir 属性获取上传的目录，最终的上传路径为 `content/{dir}`    
- 如果 dir 属性没有设置则默认上传到 content/posts 目录下    
### Text & link converters  
这块配置影响上传文章后的内容结构。  
![caef122d9c017944179ec66c06230a0d.png](/images/caef122d9c017944179ec66c06230a0d.png)  
注意：Markdown hard line break 最好关闭，我这边开启造成了空行变多，使代码难看、表格无法渲染。  
  
下来稍微麻烦点展开说说，正则内容替换：  
![e5acf60c86d000689200d0524b401b72.webp](/images/e5acf60c86d000689200d0524b401b72.webp)  
  
截图中有三个正则替换规则，每行后面都有个箭头，↑箭头表示插件应用之前替换，↓箭头表示插件应用之后替换。  
  
正则内容：  
- 第一行：图片路径，`/\]\(([^/]+?)\.(png|jpg|jpeg|webp|gif)/`  ->  `](/images/$1.$2` 且 后面的箭头向下，作用是在图片名之前加上 /images 地址前缀，我图片在本地存储的，如果你有图床那这个就不需要配置。  
- 第二行：文章之前引用，`/[^\(]+\.md/` ->  `{{</* relref "$&" >*/}}`，将 obsidian 文章之间的引用转化为 hugo 中的格式，在 obsidian 文章之间引用是没有 md 后缀的，因此执行该替换的时间是等文章使用插件转化后再执行，据此后面的箭头向下。  
- 第三行：文章封面，`/cover\.image/` -> `cover:\n    image`, 将一级 key 转为二级，因为 obsidian 不支持多层级属性。  
> `{{</* relref "例子.md" */>}}` 这个在 hugo 中表示获取“例子.md”文件的相对访问地址，例如：`例子.md` 文件的 frontmatter 的 slug 配置为 example-1，那生成的结果大概就是 `/post/example-1`，不配置 slug 那访问地址就是 `/post/例子`   
  
  
### Attachment & embed note config  
  
![1715a94838f6a513e5044787a936b518.webp](/images/1715a94838f6a513e5044787a936b518.webp)  
  
  
会将图片中使用到的本地图片上传到 `static/images` 仓库目录下。  
## 优化内容的相关插件  
  
这些 Obsidian 插件对于发布网站不是必要的，但是对于优化内容格式还是很有必要的：  
- Obsidian Linter 插件,我只用了在英文两边加空格的设置。  
- Image Converter 转化图片格式，我统一转为 webp，并设置了图片分辨率大小。  
- Unique attachments 用于将附件的文件名统一为 “字母 + 数字”的格式,记着在配置里加入 webp 图片格式  
- Image Inserter 用于找图片，我用于设置文章封面，即设置 cover:  
    image 属性。  
  
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
---  
```  
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
## 总结  
重点学会了 Github Publisher 插件后，你可以用 Hexo 等其它工具管理你的内容，只要把你 Obsidian 对应的内容放置到仓库对应的地方即可。  
  
Vercel 也不是必须的，你也可以使用 Github Page，只是我个人习惯而已。