---
layout:     post
title:      近期面试题汇总
subtitle:   Interview questions
header-style:   text
catalog: true
tags:
    - interview question
---

## 前言

我近期离职后已参加了许多场面试，因此做一个面试题的汇总记录。

## 题目

### MySql

#### MySql读写分离原理

MySql之间数据复制的基础是二进制日志文件(bin log file)，Slave数据库通过一个I/O线程与Master服务器保持通信，并监控二进制日志文件的变化，一旦文件发生变化，则会把变化复制到自己的日志中，然后SQL线程会把相关的数据库操作事件执行到自己的数据库中，依次实现从数据库和主数据库的一致性，也就是实现了主从复制。

#### MySql读写分离优势

1. 分摊服务器压力，数据库写入速度大大慢于读取速度，解决了因写入导致的读取效率问题，或者因排它锁导致的读取卡死
2. 提高可用性，当一台机器宕机时，可以调整另一台从机快速恢复服务

弊端： 增加系统复杂度或者增加了编程复杂度，主从数据表的数据一致性问题

#### MySql事务基本要素

1. 原子性（Atomicity）：事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。
2. 一致性（Consistency）：事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。
3. 隔离性（Isolation）：同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。
4. 持久性（Durability）：事务完成后，事务对数据库的所有更新将被保存到数据库，不能回滚。

#### MySql事务并发问题

1. 脏读：事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据
2. 不可重复读：事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致。
3. 幻读：系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。
4. 小结：不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表

#### MySql隔离级别

```
事务隔离级别                    脏读        不可重复读   幻读
读未提交(read-uncommitted)      是          是          是
不可重复读(read-committed)      否          是          是
可重复读(repeatable-read)       否          否          是
串行化(serializable)            否          否          否
```
mysql默认的事务隔离级别为repeatable-read

#### MySql优化

从增加索引、设计数据库、水平垂直分库分表、集群、sql优化这5个方面来优化。

可参考[MySQL优化/面试，看这一篇就够了](https://www.nowcoder.com/discuss/150059?type=0&order=0&pos=13&page=0)，[面试过程中常遇到的Mysql优化方面的面试题](https://www.2cto.com/database/201807/763914.html)

#### sql语句优化

可参考[mysql优化](https://blog.csdn.net/PHPArchitect/article/details/82108417)。

#### 一些sql语句题目

[你必知必会的SQL面试题](https://www.cnblogs.com/deng-cc/p/6515166.html)

[常见的SQL笔试题和面试题](https://blog.csdn.net/codema/article/details/80915311)

[常见的SQL笔试题和面试题](https://msd.misuland.com/pd/3255817997595449688)

[经典sql面试及答案](https://my.oschina.net/u/3706644/blog/2242223/)

#### 防止sql注入

1. 过滤参数
2. 所有sql语句使用预编译

### NoSql

#### redis的数据类型

1. String（字符串）
2. Hash（哈希）
3. List（列表）
4. Set（集合）
5. zset(sorted set：有序集合)

可参考[redis五种数据类型](https://blog.csdn.net/qq_41654985/article/details/82390875),
[redis的五种基本数据类型及其内部实现](https://blog.csdn.net/jackFXX/article/details/82318080)

#### redis常用命令

参考[redis常用命令](https://www.cnblogs.com/kevinws/p/6281395.html)

#### memcache常用命令

参考[memcached 常用命令](https://blog.csdn.net/fdipzone/article/details/8655681)

#### redis与memcache的区别

参考[redis和memcache的优缺点](https://blog.csdn.net/fdipzone/article/details/8655681)

### Nginx

#### Nginx如何与php配合工作

在linux中，nginx服务器和php-fpm可以通过tcp socket和unix socket两种方式实现。
unix socket是一种终端，可以使同一台操作系统上的两个或多个进程进行数据通信。这种方式需要再nginx配置文件中填写php-fpm的pid文件位置，效率要比tcp socket高。
tcp socket的优点是可以跨服务器，当nginx和php-fpm不在同一台机器上时，只能使用这种方式。

### Linux

#### 常用命令

参考[Linux常用命令大全](https://www.cnblogs.com/yjd_hycf_space/p/7730690.html)
参考[Linux常用命令大全](https://blog.csdn.net/ZF_9420/article/details/80466606)

#### 定时任务

参考[Linux Crontab 定时任务](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)
参考[Linux Crontab 定时任务](https://blog.csdn.net/qq_41816540/article/details/82021403)

### Vue.js

#### Vue的生命周期

参考[手把手教Vue--生命周期](https://www.jianshu.com/p/0d50ea1cef93?utm_source=oschina-app)
参考[包你理解vue生命周期](https://segmentfault.com/a/1190000014640577)

#### Vue双向绑定的原理

参考[通俗易懂了解Vue双向绑定原理及实现](https://www.cnblogs.com/wangjiachen666/p/9883916.html)
参考[vue双向绑定原理及实现](https://blog.csdn.net/gts_0901y/article/details/86028766)

#### Vue的父子组件如何通讯

参考[Vue.js 父子组件之间通信的十种方式](https://www.cnblogs.com/lhb25/p/10-way-of-vue-data-interact.html)

### Websocket

#### 怎么建立连接连接

参考[WebSocket协议解析](https://www.cnblogs.com/unclekeith/p/8087182.html)
参考[WebSocket协议](https://www.jianshu.com/p/1b2019b02126)

### Http

#### keep alive

参考[HTTP协议的Keep-Alive 模式](https://www.jianshu.com/p/49551bda6619)

#### 握手，挥手

参考[Http协议再理解（一）经典模型、三次握手、四次挥手](https://www.jianshu.com/p/bd31d3b23725)

#### Http 2.0

参考[综合阐述http1.0/1.1/2和https](https://blog.csdn.net/weixin_37719279/article/details/81388358)

### Message Queue

#### 原理

参考[再谈消息队列技术](https://www.cnblogs.com/tianqing/p/7110468.html)
参考[什么是消息队列及消息队列原理和应用场景详解](http://www.365jz.com/article/23947)

### PHP

#### 常用函数

参考[PHP常用函数大全](https://blog.csdn.net/qq_35458793/article/details/80651773)

### API设计

#### 设计要点

参考[API设计要点](https://segmentfault.com/a/1190000004503009)

#### 设计规范

参考[API 设计规范](https://www.jianshu.com/p/c2b3e55441f4)
参考[Restful API设计规范及实战](https://www.cnblogs.com/duanweishi/p/9539219.html)

#### 如何限制访问频率

使用redis记录

#### 如何提高安全性

参考[如何确保 API 的安全性](https://new.qq.com/omn/20190613/20190613A07CF800)
参考[WebApi安全性 使用TOKEN+签名验证](https://www.cnblogs.com/zxh1919/p/7670118.html)
参考[API接口的安全性](https://blog.csdn.net/weixin_43879074/article/details/89241432)

### More

#### 简述浏览器浏览网页的步骤

参考[访问一个网页的全过程](https://blog.csdn.net/u012862311/article/details/78753232)
参考[浏览器解析网页的过程](https://blog.csdn.net/u013617791/article/details/82047852)

#### 设计模式

参考[图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/)
参考[Java常见设计模式](https://www.jianshu.com/p/1c3a7ab5c527)
参考[23种设计模式总结](https://blog.csdn.net/mw_nice/article/details/82840034)

#### 跨域问题

参考[什么是跨域？跨域解决方法](https://blog.csdn.net/qq_38128179/article/details/84956552)
参考[HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

#### DNS污染

参考[什么是DNS污染](https://www.hack520.com/330.html)
参考[DNS污染应该如何解决？](https://www.jb51.net/network/643843.html)

#### CAP

参考[CAP 定理的含义](http://www.ruanyifeng.com/blog/2018/07/cap.html)
参考[分布式CAP定理，为什么不能同时满足三个特性？](https://blog.csdn.net/yeyazhishang/article/details/80758354)
