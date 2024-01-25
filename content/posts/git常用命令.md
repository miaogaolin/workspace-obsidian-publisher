---  
date: 2017-12-18 14:15:25+08:00  
tags:   
title: git常用命令  
slug: git-always-use  
share: true  
canonicalURL: https://www.jianshu.com/p/5b34566e8ea8  
keywords:   
description:   
series:   
cover:  
  image: https://images.unsplash.com/photo-1647166545674-ce28ce93bdca?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw5fHxnaXR8ZW58MHwwfHx8MTcwMzMwNTkxNHww&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
1、清除暂存区和工作区记录  
```  
git reset --hard HEAD   
git clean -df  
```  
然后运行 `git status` 显示 clean 字样说明清除成功  
![](/images/20231208091276no1.webp)  
  
2、当 a 分支修改的内容迁移到 b 分支  
  
* 回到 a 分支修改之前（假定是当前）  
```  
git reset --soft HEAD^  
```  
  
* 查看修改的文件  
```  
git status  
```  
  
* 加入到储存区  
```  
git stash  
```  
  
* 切换到新的分支并取出修改的东西  
```  
git checkout b  
git stash pop  
  
```  
  
3、从线上拉去本地不存在的分支  
  
```  
git fetch origin [远程分支]:[本地不存在的分支]  
```  
  
4、从仓库中删除文件  
```  
git rm --cached 文件路径  
```  
  
5、合并分支  
  
情况 1：假设当前分支是 master,将 develop 分支合并到 master 上  
  
```  
git merge develop  
```  
情况 2：假设当前分支是 master，将 master 分支合并到 develop 分支上  
  
6、基于分支检出 强制创建一个基于指定的 tag 的分支。  
```  
git checkout -B test v0.1.0  
```  
  
  
7、设置本地分支和远程分支的关联  
```  
git branch --set-upstream testing origin/testing  
```  
备注：设置后，在本地 testing 分支下执行 `git pull` 默认从 orgin/testing 拉取