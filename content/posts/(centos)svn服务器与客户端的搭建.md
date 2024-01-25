---  
date: 2016-09-19 17:44:29+08:00  
tags:   
title: (centos)svn服务器与客户端的搭建  
slug: centos-svn-install  
share: true  
canonicalURL: https://www.jianshu.com/p/a2ce686841ea  
keywords:   
description:   
series:   
cover:  
  image: https://images.unsplash.com/photo-1550969026-f069940eedae?q=80&w=720&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D  
---  

  
1、 yum install subversion.i686
  
2、 创建仓库
  
```
  
创建版本库目录
  
mkdir -p /home/svndata/lvmaque_svn
  
创建版本库
  
svnadmin create /home/svndata/lvmaque_svn
  
```
  
结果:
  
![Paste_Image.png](/images/20231208091246.webp)
  

  
3、进入 conf 目录（该 svn 版本库配置文件）
  

  
* authz 文件是权限控制文件
  
* passwd 是帐号密码文件
  
* svnserve.conf SVN 服务配置文件
  

  
4、设置帐号密码
  
vi passwd
  
在 [users] 块中添加用户和密码，格式：帐号=密码，如 dan=dan
  

  
5、 设置权限
  
`vi authz`
  

  
在末尾添加如下代码：
  
```
  
[/]
  
dan=rw
  
ww = r
  
```
  
意思是版本库的根目录 dan 对其有读写权限，ww 只有读权限。
  

  
- /，表示根目录及以下。根目录是 svnserve 启动时指定的，我们指定为/home/svnadmin/svndata。这样，/就是表示对全部版本库设置权限。  
  
- repos1:/，表示对版本库 1 设置权限  
  
- repos2:/occi，表示对版本库 2 中的 occi 项目设置权限  
  
- repos2:/occi/aaa,，表示对版本库 2 中的 occi 项目的 aaa 目录设置权限  
  

  
6、修改 svnserve.conf 文件
  

  
`vi svnserve.conf`
  

  
打开下面的几个注释：
  
- `anon-access = read #匿名用户可读`
  
- `auth-access = write #授权用户可写`
  
- `password-db = passwd #使用哪个文件作为账号文件`
  
- `authz-db = authz #使用哪个文件作为权限文件`
  
- `realm /home/svndata/lvmaque_svn # 认证空间名，版本库所在目录`
  

  
7、 启动服务
  

  
svnserve --help，看看这个命令的帮组，其中有 -d 和 -r，分别表示后台运行和数据仓库目录。
  

  
输入命令：
  
`svnserve -d -r /home/svndata/lvmaque_svn `-d 表示在后台运行
  

  
后面那个要跟你自己的数据仓库目录。svndata 也是自己新建的文件夹
  

  
SVN 默认监听的是 3690
  

  
8、 修改监听端口
  
```
  
svnserve --listen-port 9999 -d -r /svndata/lvmaque_svn
  
```
  
> /opt/svndata，是你的仓库地址
  

  
10、 tortoise 访问
  
选择 import,将 windows 项目中导入到 centos 中的仓库里
  
![Paste_Image.png](/images/20231208091251.webp)
  

  
`svn://192.168.1.126/svndata/lvmaque_svn`
  
**重点提醒: 配置文件前不能有空格**
  

  
11、 现在 centos 系统中有了项目仓库，然后在 apache 的服务器下检出自己的项目
  

  
![Paste_Image.png](/images/20231208091257.webp)