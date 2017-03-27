# Middleware
Sails完全兼容Express / Connect 的中间件-实际上到处都有它的身影。你在Sails写的大部分代码实际上都是中间件。尤其在[controller actions](http://sailsjs.org/documentation/concepts/Controllers?q=actions)以及[policies](http://sailsjs.org/documentation/concepts/Policies)。

<<<<<<< HEAD
### HTTP中间件
Sails也使用一个额外的[可配置中间件栈](http://sailsjs.org/documentation/concepts/Middleware?q=adding-or-overriding-http-middleware)，这个是为了HTTP请求。你的app中每次接收到的HTTP请求，配置的HTTP中间件栈井然有序地执行。
=======
Sails is fully compatible with Express / Connect middleware - in fact, it's all over the place!  Much of the code you'll write in Sails is effectively middleware; most notably [controller actions](http://sailsjs.com/documentation/concepts/Controllers?q=actions) and [policies](http://sailsjs.com/documentation/concepts/Policies).
>>>>>>> upstream/master

> 注意这个HTTP中间件栈只是用于那些“真的”HTTP请求--对于**虚拟请求**是无效的。(比如来自一条有效的socket.io连接的请求)

##### 说明:
+ `*` - 上面带有星号(*)的中间件*几乎从不*需要修改或者删除。请在你知道自己在做什么的时候才去修改或者删除它们。

<<<<<<< HEAD
#### 添加或者重写HTTP中间件
为了配置一个自定义的HTTP中间件函数，定义一个新的HTTP关键词`sails.config.http.middleware.foobar`并设置它为已经配置好的中间件函数，然后添加string名字 ("foobar")到你的`sails.config.http.middleware.order`数组中你想要在中间件链中的任何位置。(一个最好的放置位置是在"cookieParser"之前)

例子：在`config/http.js`：
=======
Sails also utilizes an additional [configurable middleware stack](http://sailsjs.com/documentation/concepts/Middleware#adding-or-overriding-http-middleware) just for handling HTTP requests.  Each time your app receives an HTTP request, the configured HTTP middleware stack runs in order.

Read about the default middleware stack [here](http://sailsjs.com/documentation/concepts/middleware/conventional-defaults).

> Note that this HTTP middleware stack is only used for "true" HTTP requests-- it is ignored for **virtual requests** (e.g. requests from a live Socket.io connection.)



#### Adding or Overriding HTTP Middleware

To configure a custom HTTP middleware function, define a new HTTP key `sails.config.http.middleware.foobar` and set it to the configured middleware function, then add the string name ("foobar") to your `sails.config.http.middleware.order` array wherever you'd like it to run in the middleware chain (a good place to put it might be right before "cookieParser"):

E.g. in `config/http.js`:
>>>>>>> upstream/master

```js
  // ...
  middleware: {

    // Define a custom HTTP middleware fn with the key `foobar`:
    foobar: function (req,res,next) { /*...*/ next(); },

    // Define another couple of custom HTTP middleware fns with keys `passportInit` and `passportSession`
    // (notice that this time we're using an existing middleware library from npm)
    passportInit    : require('passport').initialize(),
    passportSession : require('passport').session(),

    // Override the conventional cookie parser:
    cookieParser: function (req, res, next) { /*...*/ next(); },


    // Now configure the order/arrangement of our HTTP middleware
    order: [
      'startRequestTimer',
      'cookieParser',
      'session',
      'passportInit',            // <==== passport HTTP middleware should run after "session"
      'passportSession',         // <==== (see https://github.com/jaredhanson/passport#middleware)
      'bodyParser',
      'compress',
      'foobar',                  // <==== we can put this stuff wherever we want
      'methodOverride',
      'poweredBy',
      '$custom',
      'router',
      'www',
      'favicon',
      '404',
      '500'
    ]
  },

  customMiddleware: function(app){
     //Intended for other middleware that doesn't follow 'app.use(middleware)' convention
     require('other-middleware').initialize(app);
  }
  // ...
```

### 在Sails中的Express中间件
关于Sails的app中最让人高兴的是它们可以充分利用已经存在的Express/Connnect中间件的功能。但是这时人们*实际*使用它的时候都会问出这么一个问题：

<<<<<<< HEAD
> "Where do I app.use() this thing?".
=======
### Express Middleware In Sails

One of the really nice things about Sails apps is that they can take advantage of the wealth of already-existing Express/Connect middleware out there.  But a common question that arises when people _actually_ try to do this is:

> _"Where do I `app.use()` this thing?"_.

In most cases, the answer is to install the Express middleware as a custom HTTP middleware in [`sails.config.http.middleware`](http://sailsjs.com/documentation/reference/sails.config/sails.config.http.html).  This will trigger it for ALL HTTP requests to your Sails app, and allow you to configure the order in which it runs in relation to other HTTP middleware.

> You should never override or remove the `router` HTTP middleware.  It is built-in to Sails, and without it, your app's explicit routes and blueprint routes will not work.
>>>>>>> upstream/master

在大部分情况下，答案是在[`sails.config.http.middleware`](http://sailsjs.org/documentation/reference/sails.config/sails.config.http.html)中安装Express中间件为一个自定义的HTTP中间件。这将会为所有HTTP请求给你的服务器时候触发，并且允许你配置它在运行时候与其他HTTP中间件的关系的运行顺序。

<<<<<<< HEAD
### 在Sails中的Express路由中间件
你也可以包含Express中间件为策略--只需要将它配置到[`config/policies.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.policies.html)。你既可以在实际的封装策略中require和设置中间件(这算是一个好主意)或者也可以直接在你的policies.js文件中请求。下面的例子为了简单便是使用后面的方法：
=======
You can also include Express middleware as a policy- just configure it in [`config/policies.js`](http://sailsjs.com/documentation/reference/sails.config/sails.config.policies.html).  You can either require and setup the middleware in an actual wrapper policy (usually a good idea) or just require it directly in your policies.js file.  The following example uses the latter strategy for brevity:
>>>>>>> upstream/master

```js
var auth = require('http-auth');
var basic = auth.basic({
  realm: 'admin area'
}, function (username, password, onwards) {
  return onwards(username === 'Tina' && password === 'Bullock');
});

//...
module.exports.policies = {
  '*': true,

  ProductController: {

    // Prevent end users from doing CRUD operations on products reserved for admins
    // (uses HTTP basic auth)
    '*': auth.connect(basic),

    // Everyone can view product pages
    show: true
  }
}
```



<!--

  TODO:

### Advanced Express Middleware In Sails

You can actually do this in a few different ways, depending on your needs.



Generally, the following best-practices apply:

If you want a middleware function

+ If you want a piece of middleware to run only when your app's explicit or blueprint routes are matched, you should include it as a policy.
+ this will run passport for all incoming http requests, including images, css, etc.

If you want a middleware function to run for all you should include it at the top of your `config/routes.js` as a wildcard route.  for your controller (both HTTP and virtual) requests
-->






<docmeta name="displayName" value="Middleware">
