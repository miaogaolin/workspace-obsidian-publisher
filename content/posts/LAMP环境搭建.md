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
  
1. 安装 php, yum -y install php  
2. 查询是否安装了 apache `rpm -qa httpd`  
> linux 当中 apache 称为 httpd  
3. service httpd start 启动软件  
4. httpd.conf 配置文件路径 `/etc/httpd/conf/httpd.conf`  
5. systemctl   
  
> systemctl 命令是系统服务管理器指令，它实际上将 service 和 chkconfig 这两个命令组合到一起。  
  
  
### 疑点解释  
  
linux 下，源码的安装一般由 3 个步骤组成：配置（configure）、编译（make）、安装（make install）  
过程中用到 configure --prefix  --with；其中 --prefix 指的是安装路径，--with 指的是安装本文件所依赖的库文件  
  
> ./configure 的作用是检测系统配置，生成 makefile 文件，以便你可以用 make 和 make install 来编译和安装程序。  
./configure 是源代码安装的第一步，主要的作用是对即将安装的软件进行配置，检查当前的环境是否满足要安装软件的依赖关系，但并不是所有的 tar 包都是源代码的包，  
你先 ls，看有没有 configure 或者 makefile 文件。  
如果有 configure，就./configure，有很多参数。如果系统环境合适，就会生成 makefile，否则会报错。  
如果有 makefile，就直接 make，然后 make install。  
你还可以用 rpm 或者 deb 包来安装。而且现在的发行版都有自己的包管理器，比如 apt 或 yum，一个命令就可以从源下载软件，还可以自动解决依赖问题。  
1. 安装依赖包   
```bash  
yum -y install gcc  
yum -y install gcc-c++  
yum -y install make  
yum -y install perl  
```  
  
  
## apache 安装  
  
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
  
libxml2 下载地址：http://download.chinaunix.net/download/0007000/6095.shtml  
  
2. 安装 apr  
```  
gzip -d apr-1.5.2.tar.gz  
tar xvf apr-1.5.2.tar  
cd apr-1.5.2  
./configure --prefix=/usr/local/apr  
make  
make install  
```  
  
3. 安装 apr-iconv  
```  
gzip -d apr-iconv-1.2.1.tar.gz  
tar xvf apr-iconv-1.2.1.tar  
cd apr-iconv-1.2.1  
./configure --prefix=/usr/local/apr-iconv --with-apr=/usr/local/apr  
make  
make install  
```  
  
4. 安装 apr-util  
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
  
7. 配置 Apache  
  
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
添加 php 的主页  
```  
<IfModule dir_Module>  
DirectoryIndex index.html index.php  
</IfModule>  
```  
* 启动 apache 服务：  
输入命令：`/usr/local/apache/bin/apachectl start`  
  
  
8. 常见问题：  
* 通过别的机器不能访问 apache 的测试页面：http://192.168.6.888/  
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
  
* 然后保存 iptables，重启防火墙  
`[root@~]# service iptables restart`  
然后访问 `http://192.168.2.9/`（具体根据你的 ip 配置情况）  
出现“It works！”  
问题解决！！！  
  
## php 安装  
  
  
1. libxml2 安装  
```  
[root@vm15 local]# tar -zxvf libxml2-2.7.4.tar.gz   
[root@vm15 local]# cd libxml2-2.7.4  
[root@vm15 libxml2-2.7.4]# ./configure --prefix=/usr/local/libxml2  
[root@vm15 libxml2-2.7.4]# make  
[root@vm15 libxml2-2.7.4]# make install  
```  
2. php 安装  
  
```  
[root@vm15 local]# tar -xvf php-5.6.3.tar.bz2   
[root@vm15 local]# cd php-5.6.3  
[root@vm15 php-5.6.3]#./configure --prefix=/usr/local/php --with-mysql  --with-libxml-dir=/usr/local/libxml2   
```  
`--with-apxs2=/usr/local/apache2/bin/apxs` 这是我删除的，别人原本是 `./configure --prefix=/usr/local/php --with-mysql --with-apxs2=/usr/local/apache2/bin/apxs --with-libxml-dir=/usr/local/libxml2 `  
  
  
3. apache 与 php 连接配置  
  
  
1)、配置 php.ini，只需要把 php-5.6.3 安装包中的 php.ini-production 拷贝到 `/usr/local/php/lib/` 下  
  
`[root@vm15 php-5.6.3]# cp php.ini-production /usr/local/php/lib/php.ini`  
2)、配置 httpd.conf 让 apache 支持 PHP：  
  
`# vi /usr/local/apache/conf/httpd.conf`  
  
找到 (apache2.2)  
- `AddType application/x-gzip .gz .tgz` 在其下添加如下内容  
- `AddType application/x-httpd-php .php `     (.前面有空格)  
- `AddType application/x-httpd-php-source .phps`        (.前面有空格)  
  
apache2.4：  
  
- LoadModule php5_module modules/libphp5.so  (注意，在 apache 安装目录下，modules 下有 libphp5.so，这是 php 安装时添加进去的，如果没有，php，你需要重装下)  
- AddType application/x-httpd-php .php      (.前面有空格)  
　　(注意，如果上面一条没配置好的话会导致，，访问 http:localhost/*.php 会直接下载，而不是打开)  
  
3)、在 DirectoryIndex 增加 index.php，以便 Apache 识别 PHP 格式的 index  
```  
# vi /usr/local/apache/conf/httpd.conf  
  
<IfModule dir_module>    
    DirectoryIndex index.html index.php    
</IfModule>   
```  
  
# mysql 安装  
  
CentOS7 的 yum 源中默认好像是没有 mysql 的。为了解决这个问题，我们要先下载 mysql 的 repo 源。  
  
1. wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm  
2. sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm  
3. sudo yum install mysql-server  
4. init 6 重启，生成 mysql.sock   
5. 重置密码前，首先要登录 `mysql -u root`  
 > 登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql 的访问权限问题。下面的命令把/var/lib/mysql 的拥有者改为当前用户：` sudo chown -R openscanner:openscanner /var/lib/mysql`  
6. 默认登陆为 `msyql -uroot` 回车  
7. 修改密码  
```  
update user set password = PASSWORD('root') where user='root';  
FLUSH PRIVILEGES;  
```  
  
# 安装扩展库  
  
`yum install php-soap`