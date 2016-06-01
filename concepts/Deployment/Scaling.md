# 可伸缩的
如果你对你的服务器有大流量的直接需求时，你可以通过设置一个可伸缩的结构这样当越来越多人使用的时候你的app可以伸缩。

### 性能
在产品模式下，Sails表现得和Connect，Express或Socket.io app一样([example](http://serdardogruyol.com/?p=111))。如果你有自己想要分享的基准测试，请写出一篇博客或者文章然后[@sailsjs](http://twitter.com/sailsjs)。但是除了基准测试，需要牢记于心的是最重要的性能和可伸缩性的测量是专用于应用程序的。你的app的性能实际上还有很多需要去做比如你的业务逻辑实现方式和模型调用而不是你正在使用的底层框架。


### 例子框架

```
                             ....
                    /  Sails.js server  \      /  Database (e.g. Mongo, Postgres, etc)
Load Balancer  <-->    Sails.js server    <-->    Socket.io message queue (Redis)
                    \  Sails.js server  /      \  Session store (Redis, Mongo, etc.)
                             ....
```

### 配置你的服务器为一个集群部署
该记住的最重要的事情是关于一个可伸缩的服务端应用程序应该是**无状态的**。这意味着你应该能够部署相同的代码到*n*个不同的服务器上，并期望任何给定的服务器能够处理任何给定的入口请求，并且所有的一切都工作正常。幸运的是Sails app早就为这种部署做好了准备并且开箱即用。但是在你部署到多个服务器之前，还有一些东西你需要做一下：

+ 确保在你的app中你想使用的依赖软件都不会依赖于共享内存。
+ 确保你的models的数据库(比如MySQL, Postgres, Mongo)是可伸缩的(比如分区或者集群)；
+ **如果你在使用sessions**：
    + 配置你的app去使用一种共享的会话存储比如Redis(只需要在`config/session.js`文件中去掉`adapter`的注释)然后安装连接Redis的适配器作为你的app的依赖(比如`npm install connect-redis@~3.0.2 --save --save-exact`)
+ **如果你在使用SOCKETS**：
    + 配置你的app使用Redis作为一个共享消息队列，用来传递socket.io消息(只需要在`config/sockets.js`文件中去掉`adapter`的注释)
    + 安装socket.io-redis适配器作为你的app的依赖库(比如`npm install socket.io-redis@~1.0.0 --save --save-exact`)


### 部署一个Node/Sails app到PaaS
部署你的app到诸如Heroku或Modulus之类的PaaS是超级简单的。依赖于你的环境，在细节上可能会有一些差异，但是Node支持的主机提供商在过去的几年间已经变得越来越多了。更过平台专用的信息参考[Hosting](http://sailsjs.org/documentation/concepts/deployment/Hosting)。

### 部署你自己的集群

+ 在一个[负载器](https://en.wikipedia.org/wiki/Load_balancing_(computing)(比如Nginx)背后部署多个实例(也就是服务器运行你的app的副本)
    + 配置你的负载均衡器去终止SSL请求；
    + 但是记住你不需要在Sails中使用SSL配置--流量在到达Sails之前都是会被加密的。
    + 使用诸如`pm2`或`forever`之类的守护进程来启动你的每个实例(更多细节参考http://sailsjs.org/documentation/concepts/deployment)


### 优化
在你的Node/Sails中的最后一步是优化，类似于其他服务器端的应用一样；比如标识和手工优化查找慢的记录，减少查找次数等等。特别对于Node app，如果你发现在末端你有一个吃CPU很严重的流量，那么就需要查找同步的(阻塞的)模型方法，services，或者在一个循环或递归中反反复复被调用的代码。

但是记住：

> 不成熟的优化是一切恶魔的根源。--[Donald Knuth](http://c2.com/cgi/wiki?PrematureOptimization)

无论你正在使用什么工具，花费一些时间和精力去写高质量的代码，好的文档和可读性好的代码都是很重要的。那样的话，如果/当你被逼去优化你应用的一端代码，你都会发现这很容易做。

### 注意事项

> + 你不需要为你的会话非使用Redis不可--你实际上可以使用任何兼容会话存储的任何Connect或Express。更多信息参考[sails.config.session](sailsjs.org/documentation/reference/configuration/sails-config-session)
+ 默认的Socket.io配置尝试使用[长轮询](http://en.wikipedia.org/wiki/Push_technology#Long_polling)来连接服务器。为了让这个做法能够工作，你的服务器环境[必须支持](http://socket.io/blog/introducing-socket-io-1-0/#scalability)严格的负载均衡(也就是严格会话)，否则握手将会失败知道connection升级到使用WebSockets(并且仅当WebSockets可用)：
    + 在**Heroku**，你必须有严格的负载均衡beta特性[明确地使能](https://devcenter.heroku.com/articles/session-affinity)
    + 在没有严格负载均衡器的环境中，你需要[config/sockets.js](https://github.com/balderdashy/sails-docs/blob/v0.11/reference/sails.config/sails.config.sockets.md)文件中设置`transports`为`['websocket']`，强迫它只使用websocket以避免长轮询。你也需要在你的套接字客户端中设置transports--如果你正在使用`sails.io.js`，很容易在包含`sails.io.js`文件后立即添加代码`<script>io.sails.transports=['websocket']</script>`.关于这个问题的深入解释清参考[this thread](https://github.com/Automattic/engine.io/issues/261)。


<docmeta name="displayName" value="Scaling">
