---
layout:     post
title:      记使用docker构建PHP环境
subtitle:   Build PHP environment by docker
header-style:   text
catalog: true
tags:
    - docker
---

## 前言

这是我第一次使用docker。由于我对linux系统原理及命令，及php扩展安装都不熟悉，期间碰到了非常多的坑，因此做一个记录。
国内使用docker还是蛮难受的，必须使用国内docker镜像，否则很多资源拉不下来，然而拉一个100m的包还是很卡，要好久，对我测试不同版本的官方镜像造成了很大的不便。我是准备做个适用于各个php版本各个mysql版本的环境，但无奈不能一一测试了。

## 项目

[Docker_PHP](https://github.com/AnthonySAD/Docker_PHP)，该项目可以很方便的构建PHP环境，项目的具体使用方法写在了readme中，欢迎大家使用该项目。

## windows下使用docker

我一开始使用的是windows家庭版，只能按照docker toolbox，但这实在太坑了，各种问题，网上的教程也很少，千万别尝试。务必升级成windows专业版，然后安装docker windows版。

## 个人认为docker的最大优势

1. 发布开源项目时，以前还需要教大家如何安装运行环境。使用docker就不需要了，只要附上docker-compose文件即可。

2. 生产与开发环境肯定不是完全一致的，因此会引发各种bug。使用docker就可以完全统一生产与开发环境，且可以很方便的升级生产环境。

## 常用命令

- 查看docker容器进程

docker ps (-a)

- 删除容器

docker rm

docker rm -f `docker ps -aq`

- 进入容器内部

> windows下需要使用winpty
(winpty) docker exec -it 容器名 bash

- 退出容器

exit

- 删除镜像

docker rmi

docker rmi $(docker images -q)

- 生成本地镜像

docker commit 容器id 镜像名:标签名

- 推送到远程仓库

> 推送前必须登录
docker push 镜像名

- 查看所有网桥

docker network ls

## 关于docker-compose文件

- 我参考了[laradock](http://laradock.io/)的docker-compose文件的写法与风格。

- 常用的标签可以参考[Docker-Compose 配置详解](https://www.cnblogs.com/wantfly/p/9290505.html)

- php.ini等文件复制于laradock项目

- 各个容器的配置文件既可以用volume映射的方法配置，也可以在创建容器时写入。

- 日志，需要持久化的数据，本地代码等可以用volume挂载的方式，映射到docker容器中。

## 一些报错的解决方法

- error pulling image configuration: unexpected EOF

这是网络问题，修改镜像地址就好。

- Pulling from XXXX 一直不动

这是下载速度卡，耐心等待就好。如果线路不通，会报错，并结束运行的。

- 重新安装mysql或者redis镜像失败

这可能是因为，没有把本地映射的数据库文件清除。（版本不一致）

- 有时运行命令docker-compose up -d --build，容器内文件会未成功映射到本地

需要先删除所有容器，再重新up。