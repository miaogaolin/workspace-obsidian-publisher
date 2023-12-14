---  
date: 2022-01-27T15:25:26+08:00  
tags:  
  - app  
title: 2022 科学上网 Ubuntu & CFW 实现透明代理  
slug: ubuntu-cfw  
share: true  
canonicalURL:   
keywords:   
description:   
series:   
lastmod:   
---  
  
这篇文章是我折腾 3 天的结果，在这方面也是初学者，如果有问题，可以一块讨论。  
  
## 目标  
  
我家里有个不用的笔记本，所以用它来实现透明代理。  
  
<aside>  
💡  透明代理：只要连接上了家里的局域网，不管是 wifi 还是连接到路由器的宽带，都可以自动的实现科学上网，无需设置什么。  
  
</aside>  
  
## 准备  
  
**第一点**：准备一台安装了 Ubuntu 桌面的电脑，连接上你的路由器，设置有线网络为静态 ip，如下图：  
![Untitled.webp](Untitled.webp)  
![Untitled 1.webp](Untitled%201.webp)  
  
图解：  
  
- 192.168.31.1 是我的路由器地址。  
- 192.168.31.193 是我设置的 Ubuntu 的静态 ip，这个随意。  
  
**第二点**：路由器需要支持 DHCP，需要后面设置 DNS 和网关，现在暂时不需要。  
  
## 安装 CFW  
  
CFW 软件全称 clash for windows，看到名字不要以为只能在 windows 上安装，支持 mac/linux/windows，现在开始下载。  
  
**第一步**：[前往下载](https://github.com/Fndroid/clash_for_windows_pkg/releases)，选在下图红框软件。  
  
![Untitled 2.webp](Untitled%202.webp)  
  
**第二步**：解压，并进入目录运行 .`/cfw` ，这就运行起来了，但不能退出终端，有点不好。  
  
<aside>  
💡 不要在 root 下运行，会出现 —no-sandbox 的错误，找了好久也没找到解决办法，涉及 electron 和 chrome 相关的。  
  
</aside>  
  
## 加入桌面  
  
现在讲 CFW 这个软件加入 Ubuntu 的软件中心，打开终端，新建一个 **clash.desktop** 文件，写入下面信息。  
  
```bash  
[Desktop Entry]  
Name=clash for windows  
Icon=/home/miaogaolin/cfw/logo.png  
Exec=/home/miaogaolin/cfw/cfw  
Type=Application  
```  
  
重点解释：  
  
- Exec 代表刚才执行 `./cfw` 命令的路径。  
- Icon 图提供给你。  
  
![Untitled 3.webp](Untitled%203.webp)  
  
编辑好后，将该文件移动到 `~/.local/share/applications` ，完成！  
  
## 代理上网  
  
**第一步**：导入 Profiles，粘贴 clash 订阅地址，点击 download，如果前面是绿色条代表选中。  
![Profiles 界面](Untitled%204.webp)  
  
**第二步**：下来进入 General，打开如下选项，剩下的先不要管。  
![General 界面](Untitled%205.webp)  
  
图解：  
  
- Allow LAN：局域网共享。  
- Mixin：在不动原有配置情况下，增加新的配置覆盖原有配置，覆盖位置 `Settings> Profile Mixin` ，现在不用操作。  
- Start with LInux：开机自启动吧，这个我也不确定，没有测试。  
  
完成了上述步骤后，就可以进行代理上网，只需要在浏览器或者手机配置代理即可，配置信息如下：  
  
- ip: Ubuntu 系统ip  
- 端口：7890  
  
<aside>  
💡 如果不可以，可能是防火墙的问题导致端口不能访问，解决办法：[Ubuntu 开发所有端口](https://www.notion.so/Ubuntu-83f510195e89420aa23d99f7871564f7?pvs=21)  
  
</aside>  
  
## TUN  
  
### 1. 开启  
  
但这个还不算透明，还需要配置代理，现在需要开启 TUN 功能，它可以劫持代理不能做到的请求，例如：ip 直接访问。  
  
**第一步**：安装 nftables 和 iproute2。  
  
```bash  
sudo apt-get install -y nftables iproute2  
```  
  
**第二步**：点击上图 Service Mode 后的 Manage，再点击 install，我这个是已经安装好了，所以显示 Active。等一会，就把这个软件关闭重启，因为我测试时，Mac 下可以自动重启，但 Ubuntu 不行。  
![Untitled 6.webp](Untitled%206.webp)  
  
**第三步**：重启后，就可以再次打开 Manage，看是否出现 Active，或者看 Service Mode 后的是否绿色地球图标。  
![Untitled 7.webp](Untitled%207.webp)  
  
**第四步**：开启 TUN Mode，在 Connections 界面看看是否有 TUN 关键字，如果没有就等一会，因为担心没有请求。如果还不行，继续往下看解决办法。  
![Untitled 8.webp](Untitled%208.webp)  
  
### 2. 解决问题  
  
解决 TUN 不生效问题，先看 TUN 虚拟网卡是否创建，运行 `ifconfig` 命令，出现如下图：  
![Untitled 9.webp](Untitled%209.webp)  
  
或者运行 `route` 命令。  
![Untitled 10.webp](Untitled%2010.webp)  
  
为啥会有两个呢，我也没搞懂，有就对了。倘若上图你没有出现，那我也不知道，你再查查，主要是我没有遇到过。  
  
重点下来说说，满足上面的，但 TUN 还是没有生效。  
  
下来的解决办法也是猜测，但却是也成功了，我估计是 root 权限的问题。开始解决，先进入 CFW 目录。  
  
```bash  
cd resources/static/files/linux/common/service-installer  
```  
  
该目录下会出现一个 install.sh，打开后如下截图的命令没有执行成功。  
![install.sh](Untitled%2011.webp)  
  
先查看 install、nft、ip 命令是否存在。  
  
```bash  
$ which install ip nft  
/usr/bin/install  
/usr/sbin/ip  
/usr/sbin/nft  
```  
  
nft 对应的 nftables 的安装，其它的默认应该就存在，如果不存在你自己再查查。  
  
下来运行分别运行如下命令：  
  
```bash  
sudo install -m 0644 scripts/clash-default /etc/default/clash  
sudo install -m 0755 scripts/bypass-proxy-pid /usr/bin/bypass-proxy-pid  
sudo install -m 0755 scripts/bypass-proxy /usr/bin/bypass-proxy  
sudo install -m 0700 scripts/clean-tun.sh /usr/lib/clash/clean-tun.sh  
sudo install -m 0700 scripts/setup-tun.sh /usr/lib/clash/setup-tun.sh  
sudo install -m 0700 scripts/setup-cgroup.sh /usr/lib/clash/setup-cgroup.sh  
sudo install -m 0644 scripts/99-clash.rules /usr/lib/udev/rules.d/99-clash.rules  
```  
  
运行完之后再重启 CFW 再在 Connections 界面看看是否有 tun 关键字，如果没有可以在 General 界面把 TUN Mode 开关关闭再开启。  
  
这一顿折腾希望你可以整好。  
  
## 测试路由  
  
在手机上手动设置路由（网关）和 DNS，都设置为 Ubuntu 系统的 IP 地址，下来测试是否可以科学上网。  
  
如果这一步失败了，可能是 DNS 问题，可以看看这篇文章 [下载安装](https://help.bprolink.com/linux/clash.html#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85)。  
  
## DHCP  
  
我的是小米路由器，下来的配置你在自己的路由器上找找。  
![DNS 和网关](Untitled%2012.webp)  
  
图中的 ip 地址就是 Ubuntu 系统的 ip。  
  
设置好后，保存等待路由器重启，重启完成好把刚刚设置的路由和 DNS 删除，就正常连接路由器就行 。  
  
到这了，不知道你的好没好，反正我的好了，不过有啥问题可以联系我，[关于我](https://www.notion.so/9d3cf695ba0f44e787fb3f9e6aa9fd25?pvs=21) 。  
  
## 总结  
  
每个人的环境都会有差异，也会遇到各种各样的问题，所以需要极度的耐心才可以，毕竟很多人都是小白。  
  
现在也是刚开始，如果后期我有遇到新的问题，再来说说。  
  
如果想远程连接 Ubuntu 可以看看。  
  
- [Ubuntu 安装 ssh 服务](https://www.notion.so/Ubuntu-ssh-7125396394864f10a8edafebe7d0d14c?pvs=21)  
- [向日葵远程连接 Ubuntu 桌面](https://www.notion.so/Ubuntu-35799b8decc74ff49796784985d02876?pvs=21)  
  
## 参考  
  
- [TUN 模式 | Clash for Windows](https://docs.cfw.lbyczf.com/contents/tun.html#macos)  
- [DNS污染对Clash（for Windows）的影响 · Fndroid/clash_for_windows_pkg Wiki](https://github.com/Fndroid/clash_for_windows_pkg/wiki/DNS%E6%B1%A1%E6%9F%93%E5%AF%B9Clash%EF%BC%88for-Windows%EF%BC%89%E7%9A%84%E5%BD%B1%E5%93%8D)  
- [在 Ubuntu18.04 上使用 clash 部署旁路代理网关（透明代理） | Breakertt Blog](https://breakertt.moe/2019/08/20/clash_gateway/)  
- [clash配置自定义规则 - Tomorrow's blog](https://tomorrow505.xyz/clash%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99/)  
- [clash for windows允许局域网连接，TAP和TUN模式 - 旁逸斜出](https://www.mihu.live/archives/208/)  
- [Clash & Linux 实现透明代理 | 旁路网关，无需软路由/客户端让家里所有设备连接即实现科学上网 - YouTube](https://www.youtube.com/watch?v=r24-cmWV9RU)  
- [关于 clash 透明代理的一些问题 - V2EX](https://www.v2ex.com/t/691924)  
- [旁路由设置的三种方式](https://oeone.cn/archives/486.html)  
- [理解Linux虚拟网卡设备tun/tap的一切 | 骏马金龙](https://www.junmajinlong.com/virtual/network/all_about_tun_tap/)  
- [理解物理网卡、网卡接口、内核、IP等属性的关系 | 骏马金龙](https://junmajinlong.com/virtual/network/kernel_nic_ip/)  
- [udev - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/Udev)  
- [[Bug]: OpenSUSE 控制TUN模式的开关不工作](https://github.com/Fndroid/clash_for_windows_pkg/issues/2597)