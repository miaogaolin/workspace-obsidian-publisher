---  
date: 2016-09-17 23:49:35+08:00  
tags:   
title: (windows)svn服务器与客户端的搭建  
slug: win-svn-install  
share: true  
canonicalURL: https://www.jianshu.com/p/7fcba3d0236a  
keywords:   
description:   
series:   
cover:  
  image: https://images.unsplash.com/photo-1562979314-bee7453e911c?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
---  

  
1. 准备软件
  

  
VisualSVN_Server 服务器端
  
TortoiseSVN 客户端
  

  
2. 安装过程
  

  
1） 先安装好两个软件，这个没有什么难度，就不细说了
  
2） 在服务器端创建一个空仓库
  

  
要建立版本库,需要右键单击左边窗口的 Repositores,如下图:
  
![Paste_Image.png](/images/67eff45f5e396c9ad8966472d19e01ff.webp)
  
在弹出的右键菜单中选择 Create New Repository 或者新建 ->Repository:
  

  
![Paste_Image.png](/images/87b369a34baaf9a7f878c2e360da74ba.webp)
  
进入下一步，如下图
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  
点击【下一步】，如下图：
  

  
![Paste_Image.png](/images/71053e3298331446c4cea8094815dede.webp)
  

  
点击【create】，如下图：
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  
点击【Finish】即可完成基本创建。
  

  
3.  需要建立用户和组，并且需要分配权限
  

  
 3.1 在 VisualSVN Server Manager 窗口的左侧右键单击用户组,选择 Create User 或者新建 ->User,如图:
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  
点击 User 后，进入如下图：
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  
填写 Username 和 password 后，点击 ok 按钮后，进入如下图：
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  
点击上面的【Add】按钮后，如下图
  

  
![Paste_Image.png](/images/4eed15932091f69b36e814dfaf942295.webp)
  

  
增加 longen0707 到用户中 (如果有多个用户，操作一样)。
  

  
3.2  然后我们建立用户组,在 VisualSVN Server Manager 窗口的左侧右键单击用户组,选择 Create Group 或者新建 ->Group,如图:
  

  
> 建立组可以在分配权限给一个组分配，省去了多个用户相同权限的频繁操作
  

  
![Paste_Image.png](/images/67eff45f5e396c9ad8966472d19e01ff.webp)
  

  
点击【Group】按钮后，进入如下图：
  

  

  
![Paste_Image.png](/images/67eff45f5e396c9ad8966472d19e01ff.webp)
  

  
在弹出窗口中填写 Group name 为 Developers,然后点 Add 按钮,在弹出的窗口中选择 Developer,加入到这个组,然后点 Ok.
  
接下来我们需要给用户组设置权限，在 MyRepository 上单击右键,选择属性,如图:
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  
在弹出的对话框中,选择 Security 选项卡,点击 Add 按钮,选中 longen0707,然后添加进来,权限设置为 Read/Write,如下图:
  

  
![Paste_Image.png](/images/71053e3298331446c4cea8094815dede.webp)
  

  
点击【确定】按钮即可。
  

  
4. 将现有的项目导入到空仓库中
  

  
在现有的项目根目录中右键展开，选中 import
  

  
![Paste_Image.png](/images/71053e3298331446c4cea8094815dede.webp)
  

  
如下图：
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  
注意：url of repository 的填写， 59.74.104.110 是我局域网的 ip,443 是默认 visualsvn_server 的端口号，获取仓库路径的方法如下:
  

  
获取仓库路径， 在你所需要获得文件夹上右键:
  

  
![Paste_Image.png](/images/292a936aa54c954fc941e4494472e913.webp)
  

  

  

  
5. 在 visualsvn_server 下载项目
  

  
新建一个你要文件夹，保存你的项目，然后选择 checkout 检出
  

  
![Paste_Image.png](/images/71053e3298331446c4cea8094815dede.webp)
  

  
下来，如下图，url 填写服务器仓库路径：
  

  
![Paste_Image.png](/images/71053e3298331446c4cea8094815dede.webp)