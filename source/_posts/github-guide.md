---
title: github-guide
date: 2017-10-15 23:38:49
tags:
- github
- ssh
- hexo
categories:
- 编程
---

本篇文章讲述自己在搭建GitHub上所遇到的各种坑，以及常用的git命令

<!--more-->

## hexo 命令

比较靠谱的关于hexo的文档介绍

- [文档|Hexo](https://hexo.io/zh-cn/docs/index.html)
- [hexo从零开始到搭建完整 - Visugar - 博客园](http://www.cnblogs.com/visugar/p/6821777.html)
- [多电脑共管hexo博客](https://www.zhihu.com/question/21193762)

## git 命令

`git config` 用于配置全局变量，想查看有什么，可以添加 -v
`eval $(ssh-agent)`  启动代理端  `ssh-add ~/.ssh/rsa` 添加当前私钥
`git add .`  `git commit -m "some words"` 	`git push origin hexo`  将当前目录推送至远程仓库


比较靠谱的关于git的介绍

- [廖雪峰-git](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
- [一台电脑管理多个ssh](http://blog.csdn.net/qq_27376871/article/details/52485772)




