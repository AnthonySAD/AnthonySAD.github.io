---
layout:     post
title:      Git基本命令及原理
subtitle:   Git simple commands and principles
header-style:   text
catalog: true
tags:
    - git
---

## 引子

本人刚接触git的时候，只会用git clone，git pull，git add，git commit，git push等最基础的命令。当我进行高级些的操作时，git经常终止运行并输出许多我看不懂的提示，让我非常头痛。因此我决定系统得学习下git，我使用的教材是官网文档，该文档写得非常详细、配合文中的流程图可以轻松地理解git的原理。**强烈推荐**想了解git原理或者想深入学习的同学去阅读。

中文官方文档地址：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

## 起源

#### 版本控制
程序员需要监控代码的变化内容，以便将来可以历史代码及程序的迭代过程或者可以回溯代码，以防程序迭代导致的bug无法及时解决，也可找回误删的文件。所以**版本控制**是必要的，历史上出现过许多版本控制工具，但最终都被git取代。

#### git诞生
git诞生于2005年，它是由linux社区（特别是 Linux 的缔造者 Linus Torvalds）开发的。以下是git的设计原则：
- 极快的速度
- 简单的设计
- 完全分布式
- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

## 基本原理

#### 核心原理

> 了解git原理非常重要，否则很容易搞错命令导致错误。

git的机制就像虚拟机或者云服务器储存快照。每当保存时，git会创建一个快照储存每一个文件的hash索引（git使用的是SHA-1算法），并储存所有文件，但是为了提高性能，hash值一样的文件不会被重复保存。

#### 三个区域
- 工作目录 Working Directory 
    > 你编辑代码的地方
- 暂存区域 Staging Area
    > 仅仅储存了你将来会提交的文件列表
- Git仓库 Repository
    > 储存了所有快照、所有历史文件
    
#### 基本工作流程
1. 工作区修改文件
2. 将工作区修改的文件的快照添加到暂存区
3. 把暂存区的文件快照存入本地git仓库（此时，本地操作结束）
4. 把本地git仓库同步到远程git仓库

## 常用命令

克隆远程仓库:
```git
git clone 地址 本地项目名（默认和远程项目同名）
```
从远程仓库拉取（不常用）:
```git
git fetch 
```
添加工作区文件到暂存区:
```git
git add 
```
提交暂存区文件到本地仓库:
```git
git commit
```
把本地head指向的分支推送到远程仓库指定分支:
```git
git push 远程地址 分支名
```
配置git:
```git
git config
```
查看历史记录:
```git
git log
```
查看当前状态:
```git
git status
```
查看分支:
```git
git branch
```
合并分支:
```git
git merge
```
切换头指针指向:
```git
git checkout
```
切换头指针指向:
```git
git reset
```
切换头指针指向:
```git
git rebase
```


## 命令详解

git pull其实是一条组合命令,由git fetch加git merge组成.