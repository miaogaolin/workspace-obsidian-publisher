---  
date: 2021-01-14 11:06:39+08:00  
tags:   
title: Docker的cgroup讲解  
slug: docker-cgroup  
share: true  
canonicalURL: https://www.jianshu.com/p/aa4282c4ae58  
keywords:   
description:   
series:   
---  
  
最近在看一个微服务框架 `github.com/tal-tech/go-zero`，在 `core/stat/internal` 目录下学习到 cgroup 知识，本文只涉及到了我所学习到的，正文开始。  
  
---  
## 概念  
  
cgroup ，控制组，它提供了一套机制用于控制一组特定进程对资源的使用。cgroup 绑定一个进程集合到一个或多个子系统上。[官方解释](https://man7.org/linux/man-pages/man7/cgroups.7.html)  
 * subsystem，子系统，一个通过 cgroup 提供的工具和接口来管理进程集合的模块。一个子系统就是一个典型的“资源控制器”，用来调度资源或者控制资源使用的上限。其实每种资源就是一个子系统。子系统可以是以进程为单位的任何东西，比如虚拟化子系统、内存子系统。  
  
  * hierarchy，层级树，多个 cgroup 的集合，这些集合构成的树叫 hierarchy。可以认为这是一个资源树，附着在这上面的进程可以使用的资源上限必须受树上节点（cgroup）的控制。hierarchy 上的层次关系通过 cgroupfs 虚拟文件系统显示。系统允许多个 hierarchy 同时存在，每个 hierachy 包含系统中的部分或者全部进程集合。  
  
   * cgroupfs 是用户管理操纵 cgroup 的主要接口：通过在 cgroupfs 文件系统中创建目录，实现 cgroup 的创建；通过向目录下的属性文件写入内容，设置 cgroup 对资源的控制；向 task 属性文件写入进程 ID，可以将进程绑定到某个 cgroup，以此达到控制进程资源使用的目的；也可以列出 cgroup 包含的进程 pid。这些操作影响的是 sysfs 关联的 hierarchy，对其它 hierarchy 没有影响。  
  
对于 cgroup，其本身的作用只是任务跟踪。但其它系统（比如 cpusets，cpuacct），可以利用 cgroup 的这个功能实现一些新的属性，比如统计或者控制一个 cgroup 中进程可以访问的资源。举个例子，cpusets 子系统可以将进程绑定到特定的 cpu 和内存节点上。  
  
***如果未理解跳过往下看，回头再看***  
## 讲解  
1. `/proc/[pid]/cgroup` 进程的 cgroup 信息，如下图：  
![](/images/f31a639541b3609f02a2ccfee1fe9dc4.webp)  
* 每行的格式 `hierarchy-ID:controller-list:cgroup-path`，此截图中 cgroup-path 对应的容器 id  
2. `/sys/fs/cgroup/` 目录  
  
* `cpuacct/cpuacct.usage_percpu` 每个 cpu 的使用时间，如下图：  
![cpuacct.usage_percpu](/images/a8210184ed421e07a86eb59180cf5bc8.webp)  
  
* `cpuset/cpuset.cpus`cpu 列表，如下图（我的电脑是 4 核）：  
![cpuset.cpus](/images/9958ef1c6dedf4603eb92d67dcfc90b0.webp)  
  
* `cpu/cpu.cfs_period_us` 时间周期  
  
* `cpu/cpu.cfs_quota_us` 用来配置当前 cgroup 在设置的周期长度内所能使用的 CPU 时间数，两个文件配合起来设置 CPU 的使用上限。两个文件的单位都是微秒（us），cfs_period_us 的取值范围为 1 毫秒（ms）到 1 秒（s），cfs_quota_us 的取值大于 1ms 即可，如果 cfs_quota_us 的值为 -1（默认值），表示不受 cpu 时间的限制。  
  
* `cpuacct/cpuacct.usage` 统计这个 cgroup 中所有任务消耗的总 cpu 时间（纳秒）  
![cpuacct/cpuacct.usage](/images/8aa37b51455ad3c7d5ed5512972a1d3c.webp)  
  
3. **附加：**`/proc/stat` 该文件包含了所有 CPU 活动的信息，该文件中的所有值都是从系统启动开始累计到当前时刻，如下图：  
![/proc/stat](/images/3667b10b75fca213d005efb184f53561.webp)  
  
第一行代表总的 cpu 时间，单位是 jiffies，jiffies 是内核中的一个全局变量，用来记录自系统启动一来产生的节拍数，在 linux 中，一个节拍大致可理解为操作系统进程调度的最小时间片，不同 linux 内核可能值有不同，通常在 1ms 到 10ms 之间。