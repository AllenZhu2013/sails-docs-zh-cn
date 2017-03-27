# Deployment

### 在你的部署之前
在你运行任何web应用之前，你都应该问问自己几个问题：

+ 你预期中的业务量是什么？
+ 你有合约性地被要求你的服务器满足任何正常运行时间的保证吗？比如服务水平协议。
+ 哪一些的前端app会访问你的框架：

    + Android apps
    + iOS apps
    + 桌面web浏览器
    + mobile web browsers (tablets, phones, iPad minis?)
    + tvs, watches, toasters..?
+ 它们会请求哪种类型的文件？
    + JSON?
    + HTML?
    + XML?
+ 你有没有利用Socket.io的实时订阅特性？
  + 比如，聊天、实时分析、app内部的通知或消息
+ 你怎么追踪你的错误和crash？
  + 你在使用`sails.log()`吗？或者你在使用一个NPM定制的日志器比如[Winston](https://github.com/winstonjs/winston)？

### 配置你的App为产品模式
你可以使用[几个不同的方法](http://sailsjs.org/documentation/reference/configuration)提供只生效在产品模式的配置。大部分的apps发现它们自己使用一种混合式的环境变量和`config/env/production.js`。在不考虑如何操作它的情况下，本小节和文档的[Scaling](http://sailsjs.org/documentation/concepts/deployment/scaling)小节则覆盖了在你部署产品模式之前需要审查和/或者改变的配置。

### 部署单服务器
Node.js是相当快的。对于许多apps，一个服务器足以处理我们期望中的流量-至少在初期是这样的。

> 本小节关注*Sails的单服务器部署*。这种部署在规模上本质是有限的。参考[Scaling](http://sailsjs.org/documentation/concepts/deployment/scaling)一节了解关于在一个负载均衡器后面部署你的Sails/Node app。

<<<<<<< HEAD
许多团队决定部署他们的产品app在一个负载均衡器或代理后面(比如在一个PaaS如Heroku或者Modulus，或者在一个Nginx服务器后面)。这总是一个正确的办法因为在你的伸缩性需要改变并且你需要添加更多的服务器的时候它让你的app不会过时。如果你正在使用一个负载均衡器或者代理，那么下面罗列的一些事情你可以忽略掉：
=======
You can provide configuration which only applies in production in a [few different ways](http://sailsjs.com/documentation/reference/configuration).  Most apps find themselves using a mix of environment variables and `config/env/production.js`.  But regardless of how you go about it, this section and the [Scaling section](http://sailsjs.com/documentation/concepts/deployment/scaling) of the documentation cover the configuration settings you should review and/or change before going to production.
>>>>>>> upstream/master

+ 不要担心你的Sails配置为使用SSL认证。SSL几乎大部分都会在你的负载均衡器或代理服务器，或你的PaaS提供商处解析。
+ 你*也许*不需要担心设置你的app运行在端口80(如果没有配置在如Nginx之类的代理后面)。大部分的PaaS提供商都会自动地计算出合适的端口给你。如果你正在使用一个代理服务器，那么请参考它的文档(是否你需要为你的Sails app配置端口号取决于你是如何设置的以及你的需求)

> 如果你的app使用套接字并且你正在使用Nginx，确保配置它来中继websocket 消息到你的服务器。你可以在[nginx's docs on the subject](http://nginx.org/en/docs/http/websocket.html)中找到代理WebSockets的指南。

##### 设置NODE_ENV环境变量为'production'
配置你的app的环境配置为`'production'`是为了告诉Sails它面对的形势。也就是说你的app运行在一个产品的环境下。在继续执行下去之前这一步是最重要的一步。如果你在部署你的Sails app之前只有时间去改变*一个配置*，那么*这个配置肯定是这个了*。

当你的app在产品的模式下运行起来的时候：

<<<<<<< HEAD
+ 中间件和Sails依靠的其他依赖都会切换到使用更有效率地代码。
+ 你的所有[模型的迁移设置](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)被强制为`migrate: 'safe'`。这是一种失效保护，避免在部署期间出现意外导致损坏到你的产品数据。
+ 你的asset流水线运行在产品模式下。也就是意味着你的Sails app将会编译所有的样式表、客户端脚本以及预编译JST模板为压缩的`.css`和`.js`文件已减少页面加载时间和降低带宽损耗。
+ 虽然从`res.serverError()`得到的错误消息和栈追溯将会被记录，但是不会在响应中发送出去(这是为了避免攻击者访问任何敏感的信息，比如加密的密码或者在服务器的文件系统中你的Sails app所处的路径)。
=======
> This section focuses on _single-server Sails deployment_.  This kind of deployment is inherently limited in scale.  See [Scaling](http://sailsjs.com/documentation/concepts/deployment/scaling) for information about deploying your Sails/Node app behind a load balancer.
>>>>>>> upstream/master

> **注意**：
> 如果你使用别的方法设置`sails.config.environment`为`production`，那也是完全可以的。只是需要注意Sails将会自动为你设置NODE_ENV环境变量为`'production'`。这个环境变量非常重要的原因是它在Node.js中是一个通用惯例，而不管你使用的是什么框架。在Sails中内建的中间件和依赖*期望*在产品模式时`NODE_ENV`被设置--否则它们使用那些只为开发设计的最低效率的代码路径。


<<<<<<< HEAD
##### 配置你的app运行在端口80上
无论你是使用`sails_port`环境变量或在命令行选项中设置`--port`还是改变你的产品配置文件，添加下面的配置到你的Sails配置的顶层：
=======
> If your app uses sockets and you're using nginx, be sure to configure it to relay websocket messages to your server. You can find guidance on proxying WebSockets in [nginx's docs on the subject](http://nginx.org/en/docs/http/websocket.html).


##### Set the `NODE_ENV` environment variable to `'production'`

Configuring your app's environment config to `'production'` tells Sails to get its game face on; i.e. that your app is running in a production environment.  This is, hands down, the most important step. If you only have the time to change _one setting_ before deploying your Sails app, _this should be that setting_!

When your app is running in a production environment:
  + Middleware and other dependencies baked into Sails switch to using more efficient code.
  + All of your [models' migration settings](http://sailsjs.com/documentation/concepts/models-and-orm/model-settings) are forced to `migrate: 'safe'`.  This is a failsafe to protect against inadvertently damaging your production data during deployment.
  + Your asset pipeline runs in production mode (if relevant).  Out of the box, that means your Sails app will compile all stylesheets, client-side scripts, and precompiled JST templates into minified `.css` and `.js` files to decrease page load times and reduce bandwidth consumption.
  + Error messages and stack traces from `res.serverError()` will still be logged, but will not be sent in the response (this is to prevent a would-be attacker from accessing any sensitive information, such as encrypted passwords or the path where your Sails app is located on the server's file system)


>**Note:**
>If you set [`sails.config.environment`](http://sailsjs.com/documentation/reference/configuration/sails-config#?sailsconfigenvironment) to `'production'` some other way, that's totally cool.  Just note that Sails will set the `NODE_ENV` environment variable to `'production'` for you automatically.  The reason this environment variable is so important is that it is a universal convention in Node.js, regardless of the framework you are using.  Built-in middleware and dependencies in Sails _expect_ `NODE_ENV` to be set in production-- otherwise they use their less efficient code paths that were designed for development use only.




##### Configure your app to run on port 80

Whether it's by using the `sails_port` environment variable, setting the `--port` command-line option, or changing your production config file(s), add the following to the top level of your Sails config:
>>>>>>> upstream/master

```javascript
port: 80
```
> 如上面所讲的，如果你的app运行在一个负载均衡器或代理后面忽略这个步骤。

##### 为你的模型设置产品数据库
为了配置一个或多个产品数据库，可以在[`sails.config.connections`](http://sailsjs.org/documentation/reference/configuration/sails-config-connections)中配置，然后从[`sails.config.models.connection`](http://sailsjs.org/documentation/reference/configuration/sails-config-models)和/或从个别的模型中引用它们。对于大部分的app，你的产品配置改动比较简单：

1. 添加一个代表你的产品数据库的连接(比如`productionPostgresql: { ... }`)
2. 重写默认连接(`sails.config.models.connection`)，让它指向你的产品数据库(比如`productionPostgresql`)

如果你的app使用多个数据库，操作过程基本类似。但是你也许会发现重写一个已经存在的连接设置比添加一个新的连接然后修改个别的模型指向它们来得容易。忽略掉具体操作过程，如果你正在使用多个服务器，那么你应该确保当你部署为产品的时候你的模型指向正确的连接。

<<<<<<< HEAD
值得注意的是如果你使用版本控制(比如git)，那么任何敏感的认证信息(比如数据库密码)都会被上传到代码库中如果你将它们半酣在你的app的配置文件。解决这个问题的最通用的办法是将某些敏感配置设置为环境变量。更多信息请参考[Configuration](http://sailsjs.org/documentation/concepts/configuration)。
=======
To set up one or more production databases, configure them in [`sails.config.connections`](http://sailsjs.com/documentation/reference/configuration/sails-config-connections), and then refer to them from [`sails.config.models.connection`](http://sailsjs.com/documentation/reference/configuration/sails-config-models) and/or from individual models.  For most apps, your production config changes are pretty simple:
1. add a connection representing your production database (e.g. `productionPostgresql: { ... }`)
2. override the default connection (`sails.config.models.connection`) to point to your production database (e.g. `productionPostgresql`)
>>>>>>> upstream/master

如果你正在使用一个关系型数据库比如是MySQL，那么这里有一个额外步骤。还记得之前当运行在产品模式的时候Sails会设置所有的模型为`migrate:safe`?这意味着当app启动的时候不会自动迁移数据...也就是意味着默认你的数据表是不存在。解决这个的一个通常做法是在第一次启动设置一个关系数据库的时候执行下面的步骤：

<<<<<<< HEAD
+ 在产品数据库服务器上创建一个数据(比如`frenchfryparty`)
+ 本地配置你的app使用这个产品数据库，但是*不要设置环境为`'production'`并且让你的模型配置设置为`migrate: 'alter'`_*。现在运行`sails lift` **一次**-- 也就是当app加载完成之后直接关掉进程。
+ **小心！**  你应该当产品数据库*没有数据*的时候才这样做！
=======
Keep in mind that if you are using version control (e.g. git), then any sensitive credentials (such as database passwords) will be checked in to the repo if you include them in your app's configuration files.  A common solution to this problem is to provide certain sensitive configuration settings as environment variables.  See [Configuration](http://sailsjs.com/documentation/concepts/configuration) for more information.
>>>>>>> upstream/master

如果这种做法让你感到不安或者你无法远程连接到你的产品数据库，你可以忽略上面的步骤。用另外一种方法：直接dump出你的本地数据然后导入到产品数据库中去。

##### 使能CSRF保护
对于所有的Sails app来说保护CSRF是一个很重要的安全策略。如果你在开发模式的时候没有使能CSRF的话(参考[`sails.config.csrf`](http://sailsjs.org/documentation/reference/configuration/sails-config-csrf))，请确保在部署产品模式之前[使能它](http://sailsjs.org/documentation/concepts/Security/CSRF.html?q=enabling-csrf-protection)。

##### 使能SSL
如果你的API或者网站需要认证才能完成事情，那么你应该在产品模式下使用SSL。为了配置你的Sails使用SSL认证，可以使用[`sails.config.ssl`](http://sailsjs.org/documentation/reference/configuration/sails-config)。

> 如上面提到的，如果你的app运行在一个负载均衡器或者代理的后面就忽略这个步骤。

<<<<<<< HEAD

##### 启动你的App
部署的最后一个步骤就是启动服务器。比如：
=======
Protecting against CSRF is an important security measure for Sails apps.  If you haven't already been developing with CSRF protection enabled (see [`sails.config.csrf`](http://sailsjs.com/documentation/reference/configuration/sails-config-csrf)), be sure to [enable CSRF protection](http://sailsjs.com/documentation/concepts/Security/CSRF.html?q=enabling-csrf-protection) before going to production.



##### Enable SSL

If your API or website does anything that requires authentication, you should use SSL in production.  To configure your Sails app to use an SSL certificate, use [`sails.config.ssl`](http://sailsjs.com/documentation/reference/configuration/sails-config).

> As mentioned above, ignore this step if your app will be running behind a load balancer or proxy.



##### Lift Your App

The last step of deployment is actually starting the server.  For example:
>>>>>>> upstream/master

```bash
NODE_ENV=production node app.js
```

或者如果你对命令行比较熟悉的话可以使用`--prod`：

```
node app.js --prod
# (Sails will set `NODE_ENV` automatically)
```

如你所见，在产品模式下你应该使用`node app.js`来启动你的app而不是`sails lift`。通过这种方法，你的app不再依赖于`sails`命令行工具的访问权限；它只需要运行绑定在你的Sails app中的`app.js`文件。

##### ...让服务器一直运行着
除非你部署在像Heroku或Modulus这类的PaaS，否则你都需要使用一个像[`pm2`](http://pm2.keymetrics.io/)或 [`forever`](https://github.com/foreverjs/forever)之类的工具来让你的服务器能够自动启动当服务器crash之后。忽略你选择的守护进程，你应该确保它能如上面描述的那样启动服务器。

为了便利，下面给出了`pm2`和`forever`的命令行例子：

使用`pm2`:

```bash
pm2 start app.js -x -- --prod
```

使用`forever`:

```bash
forever start app.js --prod
```



<docmeta name="displayName" value="Deployment">
