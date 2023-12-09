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
  
3、进入conf目录（该svn版本库配置文件）  
  
* authz文件是权限控制文件  
* passwd是帐号密码文件  
* svnserve.conf SVN服务配置文件  
  
4、设置帐号密码  
vi passwd  
在[users]块中添加用户和密码，格式：帐号=密码，如dan=dan  
  
5、 设置权限  
`vi authz`  
  
在末尾添加如下代码：  
```  
[/]  
dan=rw  
ww = r  
```  
意思是版本库的根目录dan对其有读写权限，ww只有读权限。  
  
- /，表示根目录及以下。根目录是svnserve启动时指定的，我们指定为/home/svnadmin/svndata。这样，/就是表示对全部版本库设置权限。    
- repos1:/，表示对版本库1设置权限    
- repos2:/occi，表示对版本库2中的occi项目设置权限    
- repos2:/occi/aaa,，表示对版本库2中的occi项目的aaa目录设置权限    
  
6、修改svnserve.conf文件  
  
`vi svnserve.conf`  
  
打开下面的几个注释：  
- `anon-access = read #匿名用户可读`  
- `auth-access = write #授权用户可写`  
- `password-db = passwd #使用哪个文件作为账号文件`  
- `authz-db = authz #使用哪个文件作为权限文件`  
- `realm /home/svndata/lvmaque_svn # 认证空间名，版本库所在目录`  
  
7、 启动服务  
  
svnserve --help，看看这个命令的帮组，其中有-d和-r，分别表示后台运行和数据仓库目录。  
  
输入命令：  
`svnserve -d -r /home/svndata/lvmaque_svn `-d表示在后台运行  
  
后面那个要跟你自己的数据仓库目录。svndata也是自己新建的文件夹  
  
SVN默认监听的是3690  
  
8、 修改监听端口  
```  
svnserve --listen-port 9999 -d -r /svndata/lvmaque_svn  
```  
> /opt/svndata，是你的仓库地址  
  
10、 tortoise访问  
选择import,将windows项目中导入到centos中的仓库里  
![Paste_Image.png](/images/20231208091251.webp)  
  
`svn://192.168.1.126/svndata/lvmaque_svn`  
**重点提醒: 配置文件前不能有空格**  
  
11、 现在centos系统中有了项目仓库，然后在apache的服务器下检出自己的项目  
  
![Paste_Image.png](/images/20231208091257.webp)