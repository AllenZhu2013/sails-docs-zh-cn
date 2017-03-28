# 路由到控制器
默认Sails会为每个在控制器中的action创建一个[blueprint动作路由](http://sailsjs.org/documentation/reference/blueprint-api)，所以一个`GET`请求到`/:controllerIdentity/:nameOfAction`将会触发一个动作。在上一小节的控制器例子中，控制器被保存到`api/controllers/SayController.js`，那么`/say/hi`和`/say/bye`路由将会默认可用在Sails app运行起来的时候。如果控制器被保存到子文件夹`/we`下，那么路由将会是`/we/say/hi`和`/we/say/bye`。更多细节查看[blueprints文档](http://sailsjs.org/documentation/reference/blueprint-api)。

除了默认路由，Sails也允许使用[config/routes.js](http://sailsjs.org/documentation/concepts/Routes)文件让你手动绑定路由到控制器的动作。当你使用确切的路由的时候下面是一些参考：

+ 当你想要使用分开的动作来处理相同的路由路径，基于[HTTP方法](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。上述提及的**action blueprint**路由为一条路径到一个给定的动作绑定*所有*的请求方法，包括`GET`, `POST`, `PUT`, `DELETE`等；
+ 当你想要一个动作在一个自定义的URL上可用(比如`PUT /login`, `POST /signup`或者一条”没有意义的URL“：`GET /:username`)
+ 当你想要为如何处理路由添加额外的选项的时候(例如特殊的CORS配置)

为了在`config/routes.js`文件中手工绑定一条路由到一个控制器中，你可以使用HTTP verb和路径(也就是**路由地址**)作为关键词，并且控制器的名称+`.`+动作名称作为值(也就是**路由目标**)。

比如，下面在`api/controllers/SandwichController.js`文件中手工添加的路由会在收到一条POST请求`/make/a/sandwich`的时候，让你的服务器触发`makeIt()`动作：

```js
  'POST /make/a/sandwich': 'SandwichController.makeIt'
```

> **注意：**
>
> 对于保存在子文件夹下的控制器，子文件应该也成为控制器识别信息的一部分：
>
> ```js
>   '/do/homework': 'stuff/things/HomeworkController.do'
> ```
>
> 在`api/controllers/stuff/things/HomeworkController.js`文件中这条路由会在收到一条POST请求`/do/homework`的时候，让你的服务器触发`do()`动作。

一个完整的手工路由的讨论已经超出了这篇文档的范围--完整的可用选项概述请参考[routes documentation](http://sailsjs.org/documentation/concepts/Routes)

<docmeta name="displayName" value="Routing to Controllers">
