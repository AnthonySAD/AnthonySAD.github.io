---
layout:     post
title:      我常用的php包
subtitle:   php packages list
tags:
    - php
---

## laravel

```
composer create-project --prefer-dist laravel/laravel blog 5.7.*
```

或者使用Laravel 安装器:
```
// 安装laravel安装器
composer global require laravel/installer
//创建项目
laravel new [blog]
```

## laravel-cors
> 解决跨域请求的问题。

使用方法:
```
composer require barryvdh/laravel-cors
使用中间件\Barryvdh\Cors\HandleCors::class
```

生成配置文件:
```
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider" 
```

## laravel-ide-helper
> 解决laravel项目与phpStorm的兼容性问题

使用方法:
```
composer require --dev barryvdh/laravel-ide-helper
php artisan ide-helper:generate

//生成model文件的注释
composer require --dev doctrine/dbal
php artisan ide-helper:models Post
```

## image
> 处理图片

```
composer require intervention/image
```
使用方法见[官网地址](http://image.intervention.io/)

## predis
> 用于操作redis

```
composer require predis/predis
//重度redis使用者，推荐使用通过PECL安装PHP扩展PhpRedis，性能更好。
```
[参考文档](https://laravelacademy.org/post/19525.html)

## easyWechat
> 处理微信相关业务

```
composer require overtrue/wechat:~4.0 -vvv

//适用于laravel < 5.8
composer require "overtrue/laravel-wechat:~4.0"

//适用于laravel >= 5.8
composer require "overtrue/laravel-wechat:~5.0"
```
[官方文档](https://www.easywechat.com/docs)

## L5-swagger
> 封装了Swagger-php和swagger-ui适用于laravel5的api文档生成工具

```
composer require "darkaonline/l5-swagger:5.8.*"
```

具体使用方法请参考[我整理的文档](https://andongshen.com/2019/01/overview-swagger-and-l5-swagger.html)

[官方git地址](https://packagist.org/packages/darkaonline/l5-swagger)

## HttpClient
> 发送http请求

```
composer require guzzlehttp/guzzle
```

[官方文档](https://guzzle-cn.readthedocs.io/zh_CN/latest/index.html)

## token-generator
> 生成随机字符串

```
composer require tokenly/token-generator
```
