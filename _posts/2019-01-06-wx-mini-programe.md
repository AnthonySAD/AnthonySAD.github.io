---
layout:     post
title:      写微信小程序有感
subtitle:   Thinking about the wechat mini program
header-style:   text
catalog: true
tags:
    - wechat mini program
---

## 引子
前段时间我学习并独自开发了一个简单的微信小程序-会员卡包。目前只做了最最简单的功能，之后还会迭代。我来做一个小结。

## 后端
后端没啥好说的，没啥痛点。我使用了我熟悉的Laravel框架，并使用了swagger写api文档（只是为了试一试）。

## 前端
微信小程序前端是模仿了react和vue设计的。对我这种只玩过jquery的人蛮痛苦的。

## 微信小程序的强大
微信小程序可以获得极大的权限，包括：用户头像昵称，用户地理位置，手机的电量、wifi、陀螺仪、屏幕等状态。
小程序及其方便，不用在手机上装一大堆app。也就是说一个wechat等于n个app。

## 吐槽微信的数据绑定
vue和react都是mvvm结构的框架。react的文档我没看过，就不议论了。vue的核心功能就是数据绑定（视图模型层），可以非常爽快得以数据操控dom或以dom操作数据。但是微信小程序有点不伦不类，只能使用特定函数（page.setData()）以达到用数据操作模型的目的，且不能反向操作。比如input的value绑定了data后，input中输入值后并不能自动改变data。所以给我的感觉是，微信没有能力复制vue这类框架，只能模仿一些功能。

## 小程序的未来
google，facebook等大厂开发了pwa。pwa是非常强大的，据说是和网页开发一模一样，只是增加了一些能够操作手机的接口，所以开发速度应该比微信小程序更胜一筹。但是微信小程序的优势在于他强大的社交能力，庞大的潜在用户。