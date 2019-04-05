---
layout:     post
title:      简述swagger及l5-swagger
subtitle:   Overview swagger and l5-swagger
header-style:   text
catalog: true
tags:
    - swagger
    - l5-swagger
    - laravel
---

## 引子

在前后端分离开发的大环境下，API文档是前后端沟通的桥梁，所以一个好的API文档能大大提高工作效率。但是我在平时的开发中发现，维护一个API文档是很麻烦的事情，经常忘记更新或写的不清晰或漏写等。

为了解决以上问题，swagger诞生了。他用漂亮的ui，简单但有效的测试功能，简单的语法结构，把API文档嵌套在代码中，方便查看及修改。

## 概述

Swagger是一个基于OpenApi3.0标准的API文档编写、生成、渲染工具。l5-swagger是一个适配于Laravel集成了swagger-php和swagger-ui的包，开箱即用，非常爽。本人目前只使用laravel开发项目，所以没用过原生的swagger。以下是一些参考资料。

- [我写的使用了l5-swagger的小项目](https://github.com/AnthonySAD/wechat-membercard-api)
- [swagger-php官方demo](https://github.com/zircote/swagger-php/tree/master/Examples/petstore.swagger.io)
- [swagger官网](https://swagger.io/)
- [swagger-php官网](http://zircote.com/swagger-php/)
- [l5-swagger仓库](https://github.com/DarkaOnLine/L5-Swagger)
- [swagger文档](https://swagger.io/docs/specification/about/)
- [OpenAPI书写格式](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)

#### 核心功能

1. 根据已经编写好的yaml或json格式的文本生成好看的API文档。
2. 根据在项目注释中写的特定格式的annotation生成yaml文本。

## l5-swagger使用步骤

1. 安装
```
composer require darkaonline/l5-swagger
```

2. 创建配置文件
```
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
```

3. 开始在注释中书写Api文档

## 常用格式
> 以下格式都应用于基于OpenApi 3.0 开发的swagger 2.0

##### 文档基本信息（@OA\Info全局唯一）
```
/** 
 * @OA\Info(
 *      version="1.0.0",
 *      title="Api文档",
 *      description="文档描述",
 *      @OA\Contact(
 *          name: "API Support",
 *          url: "http://www.swagger.io/support",
 *          email="myemail@email.com"
 *      ),
 *      @OA\License(
 *          name="Apache 2.0",
 *          url="http://www.apache.org/licenses/LICENSE-2.0.html"
 *      )
 * )
 */
```

##### 额外文档
> 可用于Tag中

```
/**
 * @OA\ExternalDocumentation(
 *    description="更新日志",
 *    url="http://serve.test/doc"
 * )
 */
```

##### Api路径前缀（可多个）
```
/**
 * @OA\Server(
 *     url=“https://serve.test/api”,
 *     description="本地服务器"
 * )
 */
```

##### 接口安全中间件，L5-swagger不能使用，只能配置文件中配置
```
/**
 * @OA\SecurityScheme(
 *    type="oauth2",
 *    description="Use a global client_id / client_secret and your username / password combo to obtain a token",
 *    name="Password Based",
 *    in="header",
 *    scheme="https",
 *    securityScheme="Password Based",
 *    @OA\Flow(
 *        flow="password",
 *        authorizationUrl="/oauth/authorize",
 *        tokenUrl="/oauth/token",
 *        refreshUrl="/oauth/token/refresh",
 *        scopes={}
 *    )
 * )
 */
```

##### 标签
```
/**
 * @OA\Tag(
 *    name="订单",
 *    description="订单相关接口",
 * )
 */
```

##### 请求
> get请求

```
/**
 * @OA\Get(
 *    path="/projects",
 *    tags={"Projects"},
 *    summary="Get list of projects",
 *    description="Returns list of projects",
 *    operationId="getProject",  //全局唯一的operation的id,没啥用,可以不写
 *    @OA\Response(
 *        response=200,
 *        description="success",
 *        @OA\JsonContent(
 *            @OA\Property(
 *                property="meta",
 *                type="object",
 *                @OA\Property(property="code",type="integer"),
 *                @OA\Property(property="message",type="string"),
 *            ),
 *            @OA\Property(
 *                property="data",
 *                type="array",
 *                @OA\Items(),
 *            )
 *          )
 *      ),
 *      @OA\Response(response=400, description="Bad request"),
 * )
 */
```
> post请求

```
 /**
  * @OA\Post(
  *     path="/projects",
  *     tags={"projects"},
  *     summary="add project",
  *     security={ {"apiToken":{},"userToken":{}} }, //使用安全中间件
  *     @OA\RequestBody(
  *          description="project data",
  *          required=true,
  *          @OA\JsonContent(
  *              @OA\Property(
  *                  property="name",
  *                  type="string",
  *                  description="project name",
  *              ),
  *          )
  *     ),
  *     @OA\Response(
  *          response="200",
  *          description="Success",
  *          @OA\JsonContent(ref="#/components/schemas/success") //引用变量
  *     )
  * )
  */
```

##### 申明变量
```
 /**
  * @OA\Schema(
  *     type="",
  *     title="responseSuccess",
  *     description="请求成功",
  *     schema="success", //变量名
  *     @OA\Property(
  *         property="data",
  *         type="object",
  *     ),
  *     @OA\Property(
  *         property="code",
  *         type="integer",
  *         example=0,
  *     ),
  *     @OA\Property(
  *         property="message",
  *         type="string",
  *         example="success",
  *     )
  * )
  */
```
##### 请求参数
```
/**@OA\Parameter(
 *    name="id",
 *    description="Project id",
 *    required=true,
 *    in="path/query/cookie/header",
 *    example="1",
 *    @OA\Schema(
 *        type="integer",
 *        format="int64",
 *        minimum=1
 *    )
 * )
 */
```
##### 请求体
```
/**
 * @OA\RequestBody(
 *    description="Upload images request body",
 *    required=true,
 *    @OA\MediaType(
 *        mediaType="application/octet-stream",
 *        @OA\Schema(
 *            type="string",
 *            format="binary"
 *        )
 *    )
 * )
 */
```

##### 属性
```
/**
 * @OA\Property(
 *      property="name",
 *      type="string/array/object/integer",
 *      description="name",
 *      maxLength=30,
 *      minLength=2,
 *      default="",
 *      example="tom",
 * )
 */
```

##### 数组元素
```
/**
 * @OA\Items(
 *      type="string",
 *      description="name",
 *      maxLength=30,
 *      minLength=2,
 *      default="",
 *      example="tom",
 * )
```

## l5-swagger配置文件
> 需要个性化配置的参数主要有以下几项:

##### 修改标题
```
'api' => [
    'title' => 'L5 Swagger UI',
],
```

##### 修改Api文档路径
```
'routes' => [
    'api' => 'api/documentation',
]
```
##### 配置安全中间件
```
'security' => [
    'api_token' => [
        'type' => 'apiKey',
        'description' => '用户授权',
        'name' => 'X-AUTH-TOKEN',
        'in' => 'header',
    ],
]
```

## 总结
Swagger使用起来还是很方便的，常用的结构就这几种，用几次就熟了。
