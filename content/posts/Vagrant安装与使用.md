---  
date: 2017-09-05 21:17:35 +8000  
tags:  
  - VirtualBox  
title: Vagrant安装与使用  
slug: vagrant-install  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
---  
  
## 安装  
环境：win10  
下载 `virtualbox` 和 `vagrant`，直接傻瓜式下一步安装就行:  
* [virtualbox5.1.8](https://www.virtualbox.org/wiki/Download_Old_Builds_5_1)  
* [vagrant1.8.6](https://releases.hashicorp.com/vagrant/1.8.6/)  
  
## 常用命令  
  
|序号|命令|解释|  
|:---:|------|----------|  
|1|vagrant box list|查看目前已有的box|  
|2|vagrant box add [自定义名称] [box镜像路径]|新增加一个box|  
|3|vagrant box remove|删除指定box|  
|4|vagrant init|初始化配置vagrantfile|  
|5|vagrant up|启动虚拟机|  
|6|vagrant ssh|ssh登陆虚拟机|  
|7|vagrant suspend|挂起虚拟机|  
|8|vagrant reload|重启虚拟机|  
|9|vagrant halt|关闭虚拟机|  
|10|vagrant status|查看虚拟机状态|  
|11|vagrant destroy|删除虚拟机|  
|12|vagrant package --output xxx.box|打包分发|  
|13|vagrant package ---output xxx.box --base "自己的box"||  
  
## 应用  
1. 做好准备virtualbox、vagrant、xshell的安装工作，这里不做详细说明  
  
2. 添加box  
  
![](/images/20231208091267.webp)  
3. 查看box列表  
  
![](/images/20231208091273no1.webp)  
4. 删除box  
  
![](/images/20231208091282.webp)  
5. 初始化(在当前目录会生成`Vagrantfile`文件)  
  
![](/images/20231208091289.webp)  
  
6. 启动虚拟机  
  
![](/images/20231208091296.webp)  
**注意**: 如果启动失败，修改Vagrantfile文件  
  
![](/images/20231208091203.webp)  
  
7.  登陆虚拟机  
  
![](/images/20231208091210no1.webp)  
注意：默认用户root,密码vagrant  
  
8. 打包  
  
![](/images/20231208091217.webp)  
**注意**：  
* 打包时急着注释掉`Vargarntfile`配置文件的ip  
* 如果在vagrant up命令出现ssh连接卡死状态，则试着查看boot中是否开启 virtualox  
  
## 扩展磁盘  
  
1. 关闭实例，找到磁盘镜像文件  
  
![](/images/20231208091224no1.webp)  
  
2. 将virtualbox安装目录下的 VBoxManage命令路径添加到环境变量  
  
```  
VBoxManage.exe clonehd box-disk1.vmdk box-disk1.vdi -format VDI # 复制镜像并转化格式  
```  
  
3. 自动启动服务  
  
打开Vagrantfile文件，编辑  
```  
  config.vm.provision "shell", inline: <<-SHELL  
  #   apt-get update  
  #   apt-get install -y apache2  
      systemctl stop firewalld  
      /server/apache/bin/httpd -k start    
      /etc/init.d/mysql.server start  
  SHELL  
```  
重新启动虚拟机  
```  
vagrant reload --provision  
```  
* `--provision`表示启用上面文件编辑的配置  
  
  
  
## 问题  
  
1. 运行`vagrant up`出现  
  
![](/images/20231208091232.webp)  
  
解决办法：  
* [安裝 Hyper-V](https://docs.microsoft.com/zh-tw/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)  
* [#VirtualBox Won’t Run)](https://discuss.erpnext.com/t/virtualbox-wont-run-raw-mode-unavailable-courtesy-of-hyper-v/34541)  
  
查看以上两个文档最终解决办法是关闭Hyper-V，命令如下（使用管理员身份打开cmd）  
```  
 bcdedit /set hypervisorlaunchtype off  
```  
然后重启系统成功  
  
