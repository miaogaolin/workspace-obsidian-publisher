---  
date: 2019-07-12 09:58:16+08:00  
tags:   
title: supervisor(进程管理)小白快速上手  
slug: supervisor  
share: true  
canonicalURL: https://www.jianshu.com/p/e86d21e8864e  
keywords:   
description:   
series:   
---  
  
## 简介  
  
supervisor 是用 Python 开发的一个 client/server 服务，是 Linux/Unix 系统下的一个进程管理工具。可以很方便的监听、启动、停止、重启一个或多个进程。用 supervisor 管理的进程，当一个进程意外被杀死，supervisor 监听到进程死后，会自动将它重启，很方便的做到进程自动恢复的功能，不再需要自己写 shell 脚本来控制。  
  
  
## 安装  
  
```  
sudo apt-get install supervisor  
```  
### 其它安装方式  
使用 pip 工具  
```  
pip install supervisor  
```  
  
## 配置文件  
  
### 生成  
```  
echo_supervisord_conf > /etc/supervisord.conf  
```  
### 配置文件加载顺序  
  
默认在当前目录查找 `supervisord.conf` 配置文件  
  
1.`$CWD/supervisord.conf`  
2.`$CWD/etc/supervisord.conf`  
3.`/etc/supervisord.conf`  
4.`/etc/supervisor/supervisord.conf (since Supervisor 3.3.0)`  
5.`../etc/supervisord.conf (Relative to the executable)`  
6.`../supervisord.conf (Relative to the executable)`  
  
### 指定配置文件  
  
```  
sudo supervisord -c  supervisord.conf  
```  
  
### 配置程序  
  
增加自己的一个程序，打开 `supervisord.conf` 配置文件  
```  
[program:jshop]  
command=make run              ; the program (relative uses PATH, can take args)  
stdout_logfile=jshop.out      ; stdout log path, NONE for none; default AUTO  
```  
注意：`make run` 执行我项目中的 Makefile 文件  
其它常用程序配置  
```  
; 环境变量  
;environment=A="1",B="2"       ; process environment additions  
```  
**后续学习重点：**详细的使用说明，可以打开生成的 `supervisord.conf` 配置文件  
  
## 运行  
  
开始服务  
方式一  
```  
sudo supervisord  
```  
方式二  
```  
sudo systemctl  start supervisor  
```  
  
开始自定义的程序  
```  
sudo supervisorctl start all  
sudo supervisorctl start [自己配置的名称]  
```  
状态  
```  
sudo supervisorctl status all  
```  
停止  
```  
sudo supervisorctl stop all  
suod supervisorctl stop [自己配置的名称]  
```  
  
## 停止 supervisord 服务  
  
方式一  
```  
netstat -anp|grep super  
sudo kill [进程id]  
```  
方式二  
```  
sudo systemctl stop supervisor  
```  
如果停止后，重新启动服务，出错  
  
`Error: Another program is already listening on a port that one of our HTTP servers is configured to use.  Shut this program down first before starting supervisord.  
`  
解决办法：  
```  
sudo unlink /var/run/supervisor.sock  
sudo unlink /tmp/supervisor.sock  
```  
注：以上两种 unlink，选择自己 `supervisor.sock` 所在的位置  
  
## 总结  
  
以上在 supervisor 使用的过程中，还有其它使用方式，更详细的去往 [官网]([http://www.supervisord.org/installing.html](http://www.supervisord.org/installing.html)  
)  
  
  
