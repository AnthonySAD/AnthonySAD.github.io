---
layout:     post
title:      首次使用go构建项目-爬虫
subtitle:   First using golang to code a project, a crawler
header-style:   text
catalog: true
tags:
    - golang
    - crawler
    - my project
---

## 前言

这是我首次使用golang构建项目，也是我第一次使用编译性语言编写项目。所以该项目肯定会有许多不完美的地方。本文主要记录期间学习到的golang基础知识，及项目思路。未来会参考项目[colly](https://github.com/gocolly/colly)，写一个更完善的爬虫。

## 项目目的

1. 为了熟悉golang
2. 为了爬取一些信息
3. 我想写一个简单的搜索引擎，那么爬虫就是第一步！

## 项目地址

[go-crawler-novel](https://github.com/AnthonySAD/go-crawler-novel)

## golang基础

### 环境变量GOPATH

- 工作区目录路径
- 对于GOPATH来说，允许多个项目目录（Unix中为“：”，Windows中为“；”）

### go moudle

使用go moudle后就可以把项目放在非gopath目录下。go modulde可以对包的进行管理版本，现在官方推荐使用该方法管理包。

### ide如果无法识别项目中的import的包

可以使用go mod vendor命令把$GOPATH/pkg/mod中的包复制到项目的vendor目录下

### import使用

- 模块导入可以用相对路径，但是不推荐

- 点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名

```
import . "fmt"
```

- 别名操作就是把包命名成另一个名字

```
import f "fmt"
```

- _操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数

```
import _ "fmt"
```

### 无法下载golang.org的包

设置代理即可，参考[https://github.com/goproxy/goproxy.cn/blob/master/README.zh-CN.md](https://github.com/goproxy/goproxy.cn/blob/master/README.zh-CN.md)。如果使用的是goland，则还需在ide中设置下proxy。

### 切片常用方法

- append()函数合并切片，返回新切片

- slice[i]获取切片元素

- slice[a:b]截取切片

### 正则的坑

因为go是通过字符串生成正则表达式对象的，所以使用转义符时，得使用```\\```才行。且go语言虽然支持utf8，但是正则不支持，匹配中文的话，还是得转换成unicode才行。

### map

如果使用make方法创建了为nil的map，则该map无法使用

### interface类型

interface使go可以像动态语言一样设计和调用方法，但是写起来还是蛮麻烦的，需要通过反射库获取数据。

### goroutine

go的协程太爽了，非常轻松的实现了异步高并发。而且写起来非常简洁。

## 一些实用的包

### [gorm](https://github.com/jinzhu/gorm)

> 一个不错的ORM

[官方文档](https://gorm.io/docs/index.html)，该包使用的是github.com/go-sql-driver/mysql作为mysql连接，该包已实现了mysql连接池。可以使用SetMaxOpenConns及SetMaxIdleConns设置最大连接数，及空闲连接数。该如何合理地设置连接池大小，请参考[MYSQL调优----如何设置合理的数据库连接池的大小](https://blog.csdn.net/weixin_35794878/article/details/90342263)。

### [goquery](https://github.com/PuerkitoBio/goquery)

分析页面数据时，可以使用css选择器筛选html元素，以代替正则匹配。