---
layout:     post
title:      我构建的Laravel项目结构
subtitle:   Laravel project structure
header-style:   text
catalog: true
tags:
    - laravel
    - code layout
---

# 序

目前我在做新项目，记录下我构建项目的思路。

## 项目构建思路

当今互联网行业节奏很快，需要快速迭代，所以代码的各个功能需要解耦，每个功能都能够独立运行。为此每个功能都应该能写对应的单元测试。

Laravel的核心为IOC容器，方便代码分层，方便写单元测试。

该结构把传统MVC框架中的Controller拆为controller（只控制输入输出），service（只负责业务逻辑），repository（只负责数据库操作）。

## 命名规范

- 文件名使用大驼峰命名法（所有单词的首字母大写）
- 函数/方法/变量使用小驼峰命名法（首字母小写，第二个单词开始首字母大写），如 postCommentRecord
- 常量所有字母大写，使用_分割单词，如 PERPAGE_WECHAT_QUESTION_ANSWER
- 所有文件名,变量名,方法名,常量名使用英文全称（不要用拼音），除非是字典可查到的缩写,如no.=number。做到名称及是注释，如：addNormalQuestionAnswer()。

## 项目结构

```
|--app                          //项目核心文件
|  |--Console                   //自定义命令
|  |--Event                     //存放事件
|  |--Exception                 //异常处理
|  |  |--ApiException.php       //格式化异常输出
|  |  |--ErrorCodes.php         //自定义错误代码,及错误提示
|  |  |--Handler.php            //处理抛出的异常
|  |--Http                      //http请求/响应相关文件
|  |  |--Controllers            //存放controller文件
|  |  |--Middleware             //存放middleware文件
|  |  |--Responses              //存放响应数据格式
|  |  |  |--ApiResponse.php     //格式化返回数据(不要修改,只添加)
|  |  |--traits                 //存放controller中要用的traits
|  |  |--Kernel.php             //中间件注册(配置)文件
|  |--Listeners                 //存放监听器
|  |--Models                    //存放模型
|  |--Providers                 //注册服务提供者
|  |  |--EventServiceProvider   //注册事件和监听器
|  |  |--RouteServiceProvider   //注册路由
|  |--Repositories              //存放repository文件
|  |--Services                  //存放service文件
|  |--Utils                     //存放工具(函数,常量)
|  |  |--Constant.php           //存放常量
|  |  |--RedisUtil.php          //对redis操作的封装
|  |  |--TokenGenerator.php     //生成随机字串,如订单号,token等
|--bootstrap                    //框架缓存目录(不修改)
|--config                       //所有配置文件都存在这里
|--database                     //数据库相关文件
|  |--factories                 //生成模拟测试数据(仅测试时使用)
|  |--migrations                //数据库迁移文件
|  |--seeds                     //数据库数据初始化文件
|--public                       //入口文件夹(不修改)
|--routes                       //路由文件
|  |--api.php                   //api路由文件,之后管理端路由会分文件
|--storage                      //项目运行中生成的文件
|  |--app                       //应用相关文件
|  |  |--public                 //用户上传文件位置
|  |--logs                      //项目日志
|--tests                        //单元测试
|--.env                         //存放项目机密配置信息,如数据密码,微信secret等

```

## API文档

API文档使用了集成了swagger-php及swagger-ui的包l5-swagger。

项目的注释以openApi3.0规范编写，则能使用swagger自动生成api文档，还是比较方便的。

## 开发流程

### 创建新模块

如创建用户功能模块:

- 新建UserController.php, UserService.php, UserRepository.php并编写
- 编些路由
- 编写符合OpenApi3.0规范的注释,生成api文档
- 编写单元测试

### 创建/修改数据表

1. 创建migration文件(可使用命令php artisan make:migration)
2. 编写该文件
3. 运行migration文件(使用命令php artisan migrate)

### 创建事件监听

不需要实时生效(可延迟几秒执行)的功能应使用事件监听机制。方便后期优化，后期可以使用队列或者定时任务的方式执行，提高接口响应速度，减少数据库压力。

> 如: 短信发送,数据统计等

1. 在EventServiceProvider.php中注册事件及监听器
2. 使用命令php artisan event:generate生成文件
3. 编写方法
4. 代码中使用函数event(new  XXXevent())触发事件
