---  
date: 2016-09-27 12:09:47+08:00  
tags:   
title: LAMP环境搭建  
slug: lamp  
share: true  
canonicalURL: https://www.jianshu.com/p/ac6c9abf2240  
keywords:   
description:   
series:   
---  
  
1. 安装php, yum -y install php  
  
2. 查询是否安装了apache `rpm -qa httpd`  
>linux当中apache称为httpd  
  
3. service httpd start  启动软件  
  
4. httpd.conf配置文件路径`/etc/httpd/conf/httpd.conf`  
  
5. systemctl   
  
> systemctl命令是系统服务管理器指令，它实际上将 service 和 chkconfig 这两个命令组合到一起。  
  
  
### 疑点解释  
  
linux下，源码的安装一般由3个步骤组成：配置（configure）、编译（make）、安装（make install）  
过程中用到configure --prefix  --with；其中--prefix指的是安装路径，--with指的是安装本文件所依赖的库文件  
  
> ./configure的作用是检测系统配置，生成makefile文件，以便你可以用make和make install来编译和安装程序。  
./configure是源代码安装的第一步，主要的作用是对即将安装的软件进行配置，检查当前的环境是否满足要安装软件的依赖关系，但并不是所有的tar包都是源代码的包，  
你先ls，看有没有configure或者makefile文件。  
如果有configure，就./configure，有很多参数。如果系统环境合适，就会生成makefile，否则会报错。  
如果有makefile，就直接make，然后make install。  
你还可以用rpm或者deb包来安装。而且现在的发行版都有自己的包管理器，比如apt或yum，一个命令就可以从源下载软件，还可以自动解决依赖问题。  
1. 安装依赖包   
```bash  
yum -y install gcc  
yum -y install gcc-c++  
yum -y install make  
yum -y install perl  
```  
  
  
## apache安装  
  
下载如下包：  
apache  
http://apache.fayea.com//httpd/httpd-2.4.23.tar.gz  
  
apr  
http://apache.fayea.com//apr/apr-1.5.2.tar.gz  
  
apr-util  
http://apache.fayea.com//apr/apr-util-1.5.4.tar.gz  
  
arp-iconv  
http://apache.fayea.com//apr/apr-iconv-1.2.1.tar.gz  
  
pcre  
ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz  
  
libxml2下载地址：http://download.chinaunix.net/download/0007000/6095.shtml  
  
2. 安装 apr  
```  
gzip -d apr-1.5.2.tar.gz  
tar xvf apr-1.5.2.tar  
cd apr-1.5.2  
./configure --prefix=/usr/local/apr  
make  
make install  
```  
  
3. 安装apr-iconv  
```  
gzip -d apr-iconv-1.2.1.tar.gz  
tar xvf apr-iconv-1.2.1.tar  
cd apr-iconv-1.2.1  
./configure --prefix=/usr/local/apr-iconv --with-apr=/usr/local/apr  
make  
make install  
```  
  
4. 安装apr-util  
```  
gzip -d apr-util-1.5.4.tar.gz  
tar xvf apr-util-1.5.4.tar  
cd apr-util-1.5.4  
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr  
# ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr --with-apr-iconv=/usr/local/apr-iconv/bin/apriconv  
make  
make install  
```  
  
5. 安装 pcre  
```  
gzip -d pcre-8.39.tar.gz  
tar xvf pcre-8.39.tar  
cd pcre-8.39  
./configure --prefix=/usr/local/pcre  
make  
make install  
```  
  
6. Apache 安装  
```  
gzip -d httpd-2.4.23.tar.gz  
tar xvf httpd-2.4.23.tar  
cd httpd-2.4.23  
./configure --prefix=/usr/local/apache --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --with-apr-iconv=/usr/local/apr-iconv --with-pcre=/usr/local/pcre --enable-so  
make  
make install  
```  
  
7. 配置Apache  
  
* 在安装的目录下修改文件：  
命令：`vi /usr/local/apache/conf/httpd.conf`  
把：  
`# ServerName http://www.example.com:80`  
改为：  
`ServerName localhost:80`  
  
* 配置自己的发布主页目录  
```  
DocumentRoot "/usr/local/httpd/htdocs"  
<Directory "/usr/local/httpd/htdocs">  
```  
添加php的主页  
```  
<IfModule dir_Module>  
DirectoryIndex index.html index.php  
</IfModule>  
```  
* 启动apache服务：  
输入命令：`/usr/local/apache/bin/apachectl start`  
  
  
8. 常见问题：  
* 通过别的机器不能访问apache的测试页面：http://192.168.6.888/  
一般是防火墙配置的问题。  
  
解决方法：  
`[root@~]# vi /etc/sysconfig/iptables`  
添加如下代码在“`:OUTPUT ACCEPT [0:0]`”之后。  
  
```  
:OUTPUT ACCEPT [0:0]  
-A OUTPUT -p tcp --sport 80 -j ACCEPT  
-A INPUT -p tcp --dport 80 -j ACCEPT  
```  
* 说明  
  
sport 指定匹配规则的源端口   
  
dport 指定匹配规则的目的端口  
  
OUTPUT 处理出站信息  
  
INPUT 处理入站信息  
  
* 然后保存iptables，重启防火墙  
`[root@~]# service iptables restart`  
然后访问 `http://192.168.2.9/`（具体根据你的ip配置情况）  
出现“It works！”  
问题解决！！！  
  
## php安装  
  
  
1. libxml2安装  
```  
[root@vm15 local]# tar -zxvf libxml2-2.7.4.tar.gz   
[root@vm15 local]# cd libxml2-2.7.4  
[root@vm15 libxml2-2.7.4]# ./configure --prefix=/usr/local/libxml2  
[root@vm15 libxml2-2.7.4]# make  
[root@vm15 libxml2-2.7.4]# make install  
```  
2. php安装  
  
```  
[root@vm15 local]# tar -xvf php-5.6.3.tar.bz2   
[root@vm15 local]# cd php-5.6.3  
[root@vm15 php-5.6.3]#./configure --prefix=/usr/local/php --with-mysql  --with-libxml-dir=/usr/local/libxml2   
```  
`--with-apxs2=/usr/local/apache2/bin/apxs`这是我删除的，别人原本是 `./configure --prefix=/usr/local/php --with-mysql --with-apxs2=/usr/local/apache2/bin/apxs --with-libxml-dir=/usr/local/libxml2 `  
  
  
3. apache与php连接配置  
  
  
1)、配置php.ini，只需要把php-5.6.3安装包中的php.ini-production拷贝到`/usr/local/php/lib/`下  
  
`[root@vm15 php-5.6.3]# cp php.ini-production /usr/local/php/lib/php.ini`  
2)、配置 httpd.conf 让apache支持PHP：  
  
`# vi /usr/local/apache/conf/httpd.conf`  
  
找到 (apache2.2)  
- `AddType application/x-gzip .gz .tgz` 在其下添加如下内容  
- `AddType application/x-httpd-php .php `     (.前面有空格)  
- `AddType application/x-httpd-php-source .phps`        (.前面有空格)  
  
apache2.4：  
  
- LoadModule php5_module modules/libphp5.so  (注意，在apache安装目录下，modules下有libphp5.so，这是php安装时添加进去的，如果没有，php，你需要重装下)  
- AddType application/x-httpd-php .php      (.前面有空格)  
　　(注意，如果上面一条没配置好的话会导致，，访问http:localhost/*.php会直接下载，而不是打开)  
  
3)、在DirectoryIndex增加 index.php，以便Apache识别PHP格式的index  
```  
# vi /usr/local/apache/conf/httpd.conf  
  
<IfModule dir_module>    
    DirectoryIndex index.html index.php    
</IfModule>   
```  
  
# mysql安装  
  
CentOS7的yum源中默认好像是没有mysql的。为了解决这个问题，我们要先下载mysql的repo源。  
  
1. wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm  
2. sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm  
3. sudo yum install mysql-server  
4. init 6 重启，生成mysql.sock   
5. 重置密码前，首先要登录`mysql -u root`  
 > 登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：` sudo chown -R openscanner:openscanner /var/lib/mysql`  
6. 默认登陆为`msyql -uroot`回车  
7. 修改密码  
```  
update user set password = PASSWORD('root') where user='root';  
FLUSH PRIVILEGES;  
```  
  
# 安装扩展库  
  
`yum install php-soap`