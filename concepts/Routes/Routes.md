# Routes
### 概述
任何web应用的最基本的特性就是能够拦截发送给一个URL的请求，然后返回一个响应。为了实现这个，你的应用必须能够区分不同的URL。

就像大部分web框架一样，Sails提供一个路由器：一种映射URL到控制器和视图的机制。**Routes**就是告诉Sails当面对一条进入的请求是该做些什么的规则。在Sails中有两种主要类型的路由：**自定义**(或者“明确的”)和**自动的**(或者“不明确的”)。

### 自定义路由
Sails让你自己用你自己喜欢的方法设计你的app的URLs--也就是没有框架限制。

每一个Sails的工程都带有[`config/routes.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.routes.html)，一个简单的[Node.js module](http://nodejs.org/api/modules.html)，可以到处一个自定义的对象或者明确的**routes**。比如，下面的这个`routes.js`文件定义了6条路由；它们有些指向一个控制器的动作，其他的一些直接指向视图。

```javascript
// config/routes.js
module.exports.routes = {
  'get /signup': { view: 'conversion/signup' },
  'post /signup': 'AuthController.processSignup',
  'get /login': { view: 'portal/login' },
  'post /login': 'AuthController.processLogin',
  '/logout': 'AuthController.logout',
  'get /me': 'UserController.profile'
}
```

每一条**route**都包含一个**地址**(最左边的字符串，比如`“get /me”`)和一个**目的地**(最右边的字符串，比如`'UserController.profile'`)。这个**地址**是一个URL路径并且(可选地)带有一个专用的[HTTP方法](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)。**目标**可以被定义为各种不同的方法(参考the expanded concepts section on the subject](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html)t)，但是上面的两种不同的方法是最常见的。当Sails收到一条请求时，它会检测所有自定义的路由的**地址**并找到匹配的那条。如果匹配的路由找到了那么将会将这条请求传给**目标**。

比如，我们读取`'get /me': 'UserController.profile'`会是这样子的：

>  "Hey Sails, when you receive a GET request to `http://mydomain.com/me`, run the `profile` action of `UserController`, would'ya?"

如果我们想改变路由本身自己的视图布局时该怎么办？很简单，只需这样：

```javascript
'get /privacy': {
    view: 'users/privacy',
    locals: {
      layout: 'users'
    }
  },
```

### 注意：

+ Just because a request matches a route address doesn't necessarily mean it will be passed to that route's target _directly_.  For instance, HTTP requests will usually pass through some [middleware](http://sailsjs.org/documentation/concepts/Middleware) first.  And if the route points to a controller [action](http://sailsjs.org/documentation/concepts/Controllers?q=actions), the request will need to pass through any configured [policies](http://sailsjs.org/documentation/concepts/Policies) first.  Finally, there are a few special [route options](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options) which allow a route to be "skipped" for certain kinds of requests.
+ The router can also programmatically **bind** a **route** to any valid route target, including canonical Node middleware functions (i.e. `function (req, res, next) {}`).  However, you should always use the conventional [route target syntax](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html) when possible- it streamlines development, simplifies training, and makes your app more maintainable.

### 自动化的路由
除了你自定义的路由，Sails还自动地帮你绑定了许多路由。如果一条URL不匹配任何自定义的路由，它可能会匹配上自动路由中的一条，那么还是会生成一条响应的。在Sails中主要类型的自动化路由如下：

+ [Blueprint routes](http://sailsjs.org/documentation/reference/blueprint-api?q=blueprint-routes)，提供一个完整的REST API给你的[controllers](http://sailsjs.org/documentation/concepts/Controllers)和[models](http://sailsjs.org/documentation/concepts/ORM/Models.html)；
+ [Assets](http://sailsjs.org/documentation/concepts/Assets)： 比如图片、JS和CSS文件
+ [CSRF](http://sailsjs.org/documentation/concepts/Security/CSRF.html)： 如果打开的话，将会在你的app中提供一条**/csrdfToken**路由，这样可以用于取回CSRF token。

### 支持的协议
Sails路由器是“protocol-agnostic”；它知道如何处理通过[Websockets](http://en.wikipedia.org/wiki/Websockets)的[HTTP请求](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)和消息。它通过监听Socket.io消息来实现，该消息以一种简单的格式发送给保留的event handlers，该格式称为JWR(JSON-Websocket Request/Response)。详细说明和实现参考现在可用的文档[client-side socket SDK](http://sailsjs.org/documentation/reference/websockets/sails.io.js)。

#### 注意
+ 高级用户也许会完全地绕过路由器然后直接地发送底层的完全自定义的WebSocket消息给底层的Socket.io服务器。你可以在你的app的[`onConnect`](http://sailsjs.org/documentation/reference/sails.config/sails.config.sockets.html?q=commonlyused-options)函数(位于[`config/sockets.js`](http://sailsjs.org/documentation/anatomy/myApp/config/sockets.js.html)文件中)中直接绑定你的套接字事件。但是记住在大部分的情况下，你最好放弃利用请求拦截器来进行套接字通信--因为在HTTP和WebSockets之间维护一直的路由会让你的app变得更加可维护性。



<docmeta name="displayName" value="Routes">
