---  
date: 2016-09-17 23:49:35 +8000  
tags:   
title: (windows)svn服务器与客户端的搭建  
slug: win-svn-install  
share: true  
canonicalURL: https://www.jianshu.com/p/7fcba3d0236a  
keywords:   
description:   
series:   
---  
  
1. 准备软件  
  
VisualSVN_Server     服务器端  
TortoiseSVN             客户端  
  
2. 安装过程  
  
1） 先安装好两个软件，这个没有什么难度，就不细说了  
2） 在服务器端创建一个空仓库  
  
要建立版本库,需要右键单击左边窗口的Repositores,如下图:  
![Paste_Image.png](/images/20231208091289no2.webp)  
在弹出的右键菜单中选择Create New Repository或者新建->Repository:  
  
![Paste_Image.png](/images/20231208091295.webp)  
进入下一步，如下图  
  
![Paste_Image.png](/images/20231208091204.webp)  
点击【下一步】，如下图：  
  
![Paste_Image.png](/images/20231208091210.webp)  
  
点击【create】，如下图：  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
点击【Finish】即可完成基本创建。  
  
3.  需要建立用户和组，并且需要分配权限  
  
 3.1 在VisualSVN Server Manager窗口的左侧右键单击用户组,选择Create User或者新建->User,如图:  
  
![Paste_Image.png](/images/20231208091204.webp)  
点击User后，进入如下图：  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
填写Username和password后，点击ok按钮后，进入如下图：  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
点击上面的【Add】按钮后，如下图  
  
![Paste_Image.png](attachments/Paste_Image.png-1.webp)  
  
增加longen0707到用户中(如果有多个用户，操作一样)。  
  
3.2  然后我们建立用户组,在VisualSVN Server Manager窗口的左侧右键单击用户组,选择Create Group或者新建->Group,如图:  
  
> 建立组可以在分配权限给一个组分配，省去了多个用户相同权限的频繁操作  
  
![Paste_Image.png](/images/20231208091289no2.webp)  
  
点击【Group】按钮后，进入如下图：  
  
  
![Paste_Image.png](/images/20231208091289no2.webp)  
  
在弹出窗口中填写Group name为Developers,然后点Add按钮,在弹出的窗口中选择Developer,加入到这个组,然后点Ok.  
接下来我们需要给用户组设置权限，在MyRepository上单击右键,选择属性,如图:  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
在弹出的对话框中,选择Security选项卡,点击Add按钮,选中longen0707,然后添加进来,权限设置为Read/Write,如下图:  
  
![Paste_Image.png](/images/20231208091210.webp)  
  
点击【确定】按钮即可。  
  
4. 将现有的项目导入到空仓库中  
  
在现有的项目根目录中右键展开，选中import  
  
![Paste_Image.png](/images/20231208091210.webp)  
  
如下图：  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
注意：url of repository的填写， 59.74.104.110是我局域网的ip,443是默认visualsvn_server的端口号，获取仓库路径的方法如下:  
  
获取仓库路径， 在你所需要获得文件夹上右键:  
  
![Paste_Image.png](/images/20231208091204.webp)  
  
  
  
5. 在visualsvn_server下载项目  
  
新建一个你要文件夹，保存你的项目，然后选择checkout检出  
  
![Paste_Image.png](/images/20231208091210.webp)  
  
下来，如下图，url填写服务器仓库路径：  
  
![Paste_Image.png](/images/20231208091210.webp)