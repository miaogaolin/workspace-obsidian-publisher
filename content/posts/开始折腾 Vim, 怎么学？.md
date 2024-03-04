---
date: 2024-03-04T12:00:03+08:00
tags:
  - app_vim
title: 开始折腾 Vim, 怎么学？
slug: vim-study
share: true
canonicalURL: 
keywords:
  - vim
  - vscode
  - vimtutor
  - Vim实用技巧
  - vimgolf
description: 
series: 
lastmod: 
cover:
    image: https://images.unsplash.com/photo-1608452964553-9b4d97b2752f?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwzMHx8bGludXh8ZW58MHwwfHx8MTcwOTUyNDg3OXww&ixlib=rb-4.0.3&q=80&w=700
author: 
dir: posts
---


Vim 从我知道开始最少八年了，而一直知道的命令数量反正不超过手指头。反正还到不了我用它写代码的地步，只是偶尔在 Linux 上改改文件。

今天突然想开始系统学习学习，我估摸的原因：
1. 我进入这个行业时间久，接触到很多人也在学习，感觉自己不学的话，有点不好意思
2. 我用过的很多编辑器都集成了 Vim，例如 vscode、obsidian，感受操作的统一性
3. 想感受下学会了 Vim 是如何提高效率的

## 怎么学
按照下面的资料顺序学习就行。

### 视频
Vim 学起来还是挺枯燥的，我还是觉得先看看好点的视频讲解。

我推荐这个 [Vscode + Vim](https://www.bilibili.com/video/BV1z541177Jy/?vd_source=1421aab889aa8c67977f9d6f83271fdc) 怎么搭配使用的讲解，当然重头还是讲 Vim 怎么使用。


### vimtutor
适合刚开始接触入门。

有 Vim 工具，就一定会配套一个 vimtutor 命令，这是官方的 Vim 文档，也支持中文。

使用该工具的好处是，**边看文档变练习**。

命令行运行：
```bash
vimtutor
```
如果打开的内容不是中文的，可以运行：
```
vimtutor zh
```

### 更进一步
上面的学完了，如果还想更系统的再学习学习，推荐下面资料。
1. 《Vim实用技巧》这本书等上面学完了，可以通过这本书再巩固学习，而且也确实写的好，对于提高Vim 的使用速度很有帮助。
2. [《Learn Vimscript the Hard Way》](https://learnvimscriptthehardway.stevelosh.com/)：想写 Vim 插件，可以看看。
3. [VimGolf](https://www.vimgolf.com/) ：通过挑战，类似玩游戏的方式学习 Vim，可以看别人针对某个目标是如何实现的，你也可以提交自己实现的结果，像做题一样，有打分机制。


## Golang 开发环境
如果你想纯 Shell 端开发环境，推荐看这篇文章：[如何配置 Vim 的 Golang 开发环境](https://taoshu.in/vim/go-vim.html)。


## 总结
假如你像我一样，平时都在 IDE 开发项目，那基本都会支持 Vim 的。

先学会 Vim 的各种键位，看是否自己能坚持下来，或者明确自己是否喜欢，当过这个都坚持不下来，那确实不适合你。

目前我还在 Vscode + Vim 这种配合下面挣扎，编写代码的效率现在还很差，最终会不会又放弃，还不得而知😂。

## 参考
- [我和VIM的故事 | Henry Z's blog~](https://changchen.me/blog/20180223/vim-tour/)
