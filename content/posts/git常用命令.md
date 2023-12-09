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
---

1、清除暂存区和工作区记录
```
git reset --hard HEAD 
git clean -df
```
然后运行`git status`显示clean字样说明清除成功
![](/images/20231208091276no1.webp)

2、当a分支修改的内容迁移到b分支

* 回到a分支修改之前（假定是当前）
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

情况1：假设当前分支是master,将develop分支合并到master上

```
git merge develop
```
情况2：假设当前分支是master，将master分支合并到develop分支上

6、基于分支检出 强制创建一个基于指定的tag的分支。
```
git checkout -B test v0.1.0
```


7、设置本地分支和远程分支的关联
```
git branch --set-upstream testing origin/testing
```
备注：设置后，在本地testing分支下执行`git pull`默认从orgin/testing拉取