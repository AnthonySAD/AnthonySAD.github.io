---
layout:     post
title:      Git基本命令及原理
subtitle:   Git simple commands and principles
header-style:   text
catalog: true
tags:
    - git
---
> 本文是我原创的，转载请注明出处

> 由于我是初学者，以后有了新理解后，我还会更新本文

## 引子

本人刚接触git的时候，只会用git clone，git pull，git add，git commit，git push等最基础的命令。当我进行高级些的操作时，git经常终止运行并输出许多我看不懂的提示，让我非常头痛。因此我决定系统得学习下git，我使用的教材是官网文档，该文档写得非常详细、配合文中的流程图可以轻松地理解git的原理。**强烈推荐**想了解git原理或者想深入学习的同学去阅读。

中文官方文档地址：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

## 起源
### 版本控制
程序员需要监控代码的变化内容，以便将来可以历史代码及程序的迭代过程或者可以回溯代码，以防程序迭代导致的bug无法及时解决，也可找回误删的文件。所以**版本控制**是必要的，历史上出现过许多版本控制工具，但最终都被git取代。

### git诞生
git诞生于2005年，它是由linux社区（特别是 Linux 的缔造者 Linus Torvalds）开发的。以下是git的设计原则：

- 极快的速度
- 简单的设计
- 完全分布式
- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

## 最新版安装

从 [https://github.com/git/git/releases](https://github.com/git/git/releases)下载最新稳定版的tar.gz包。

```shell
tar zxvf git.tar.gz
cd git.tar.gz
autoconf
./configure
make
make install
export PATH=$PATH:/usr/local/bin
```

## 基本原理

### 核心原理

> 了解git原理非常重要，否则很容易搞错命令导致错误。

git的机制就像虚拟机或者云服务器储存快照。每当保存时，git会创建一个快照储存每一个文件的hash索引（git使用的是SHA-1算法），并储存所有文件，但是为了提高性能，hash值一样的文件不会被重复保存。

### 三个区域
- 工作目录 Working Directory 
    > 你编辑代码的地方
- 暂存区域 Staging Area
    > 储存了你将下次提交时的文件状态
- Git仓库 Repository
    > 储存了所有快照、所有历史文件

### 基本工作流程
1. 工作区修改文件
2. 将工作区修改的文件的快照添加到暂存区
3. 把暂存区的文件快照存入本地git仓库，并把修改的文件存入git仓库（此时，本地操作结束）
4. 把本地git仓库同步到远程git仓库

## 常用命令
> 以下仅仅列出了常用命令

> ?[]表示可选参数

初始化本地仓库:
```git
git init
```
克隆远程仓库:
```git
git clone [remote repository] ?[local folder]
```
从远程仓库拉取（不常用）:
```git
git fetch ?[remote repository] ?[remote branch]
```
添加工作区文件到暂存区:
```git
git add [files list]
```
提交暂存区文件到本地仓库:
```git
git commit -m 'message'
```
添加工作区文件到暂存区并提交到本地仓库:
```git
git commit -am 'message'
```
编辑上一次提交记录，并重新提交:
```git
git commit --amend
```
把本地分支推送到远程仓库:
> 如果不写参数，则默认推送到追踪的远程分支。如果不写remote branch，则推送到与本地分支名一样的分支（哪怕已经追踪了其他远端分支）。如果不写本地分支名，写了远端分支名，表示删除远端分支。

```git
git push ?[remote repository] ?[local branch]:[remote branch]
```
删除远程分支:
```git
git push [remote repository] --delete [remote branch]
```
配置git:
```git
git config
```
查看历史记录（后面有详解）:
```git
git log
```
查看当前状态:
```git
git status
```
查看代码变化:
```git
git diff ?[branch|commit hash] ?[branch|commit hash]
```
查看本地分支列表:
```git
git branch 
```
查看所有分支列表:
```git
git branch -a/--all
```
查本地分支列表，并显示对应的跟踪的远程分支状态:
```git
git branch -vv/-v
```
合并分支:
```git
git merge [branch]
```
切换头指针指向（后面有详解）:
```git
git checkout
```
> 以下是tag相关的命令，我没用过，所以记录的比较详细

查看标签列表:
```git
git tag
```
添加轻量级标签:
```git
git tag [tag name] ?[commit hash]
```
添加有注释的标签:
```git
git tag -a [tag name] -m [annotation]
```
查看标签详情:
```git
git show [tag name]
```
推送标签:
```git
git push [remote repository] [tag name]
```
推送全部标签:
```git
git push [remote repository] --tags
```
删除本地标签:
```git
git tag -d [tag name]
```
删除远程标签:
```git
git push [remote repository] :refs/tags/[tag name]
```
> 以下是不太常用得命令

切换头指针并充值工作区，暂存区（后面有详解）:
```git
git reset
```
变基:
```git
git rebase
```
给命令起个别名:
```git
git alias
如：git config --global alias.ll 'log --oneline'
```
查看git仓库中的对象:
```git
git cat-file -t/-p [hash]
```

## 命令详解

### 语法糖命令
> 我把**一条等价于几条命令组合的命令**称为语法糖命令
> 以下是我对一些命令的理解

#### git clone
git clone [remote repository] [local folder]等价于：
1. mkdir [local folder] && cd [local folder]
> 创建本地文件夹并进入

2. git init
> 初始化本地仓库

3. git remote add origin [remote repository]
> 本地添加远程仓库地址，并起别名叫origin

4. git fetch
> 拉取远程仓库所有数据存入本地仓库
> （注意：此时本没有任何分支，工作区也是空的）

5. git checkout master
> - 创建本地master分支
> - HEAD指向该分支
> - 远程master数据复制到本地暂存区和工作区。
> - 本地master追踪远程master

#### git pull
git pull [remote repository] [branch name]等价于:
1. git fetch [remote repository] [branch name]
> 拉取远程仓库
2. git merge 
> 合并远程分支到本地分支

#### git checkout
git checkout -b [branch name]等价于:
1. git branch [branch name]
> 创建新分支
2. git checkout [branch name]
> 切换到该分支

### 高级命令

#### git log
git log非常有用，可以查看历史提交记录，及HEAD指向。。 一共有几十个选项，且可以组合使用。
[**官方文档**](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)列了许多常用的选项。

以下只介绍我用过的：
- --oneline   每个commit仅显示一行信息
- -n              显示最近的n条commit
- --merges  仅查看merge commit
- -p              查看详细的每个commit的代码变更情况
- --since      只显示距今多久的commit
> 如: git log --since=2hours.30minutes 仅仅查看距今2个半小时的commit
- --since    只显示多久之前的commit

#### git checkout
- git checkout [file name]
把暂存区的某文件复制到工作区
- git checkout [commit hash] [file name]
把某个commit的某文件复制到工作区
- git checkout --orphan [new branch]
创建一个当前分支的镜像，但是没有任何提交记录，一般用于清除旧分支的提交信息

#### git rebase
- git rebase -i HEAD~n
修改前n次的提交记录，可以修改，可以合并提交等操作
- git rebase --continue
有时候需要解决合并冲突，解决完后需要运行以上命令

#### git reset
- git reset HEAD~
撤销上一次的提交，修改了仓库和暂存区，工作区不变
- git reset HEAD~ --soft
撤销上一次的提交，修改了仓库，暂存区和工作区不变
- git reset HEAD~ --soft
撤销上一次的提交，仓库，暂存区，工作区全部修改
> 以下是复制官方文档的一个小节:

下面的速查表列出了命令对树的影响。 “HEAD” 一列中的 “REF” 表示该命令移动了 HEAD 指向的分支引用，而`‘HEAD’' 则表示只移动了 HEAD 自身。 特别注意 WD Safe? 一列，如果它标记为 NO，那么运行该命令之前请考虑一下。

![image](/img/posts/git-simple-commands-and-principles/reset-and-checkout.png)

## 个人常用命令缩写

```
ll = git log --oneline
ac = git commit -am
```

## 总结
个人感觉暂存区是工作区与仓库之间的中间件，最早是为了程序员可以选择性地git add文件，只让gti跟踪某些文件。但是由于这样操作太麻烦了（或者以前可能也考虑到了计算机的性能问题），现在只要把git追踪的文件放在.gitignore里就能放心得提交了。
现在暂存区的主要用处是防止程序员误操作，且可以辅助程序员检查代码。

git命令非常丰富，但是我们只需要掌握以上命令就能满足日常工作需求了。并且只要明白了git的工作原理，碰到问题时也不会手忙脚乱，也不会因为害怕删除代码而不敢尝试去解决问题了。
