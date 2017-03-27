# Views
### 概述
在Sails中视图是标记模板，在*服务器上*被编译成HTML页面。在大部分的情况下，视图被用来响应入口的HTTP请求，比如展示你的主页。

或者视图也可以直接被编译成一段HTML字符串用在你的后台代码上(参考[`sails.renderView()`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md))。比如你可能使用这种方法去发送HTML邮件或者构建给传统API使用的大XML字符串。

##### 创建一个视图
默认的，Sails使用EJS([Embedded Javascript](http://embeddedjs.com/))作为它的模板引擎。EJS的语法是非常传统的--如果你曾经使用过php, asp, erb, gsp, jsp等，那么你就可以直接上手的。

如果你倾向于使用一个不同的模板引擎，这里有很多种选择。Sails支持所有与[Express](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)兼容的模板引擎通过[Consolidate](https://github.com/visionmedia/consolidate.js/)。

视图默认的定义在你的[`views/`](http://sailsjs.org/documentation/anatomy/myApp/views)文件夹下，就像其他所有默认的路径一样，它们都是可以[configurable](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)。如果你一点儿也不需要动态HTML页面(比如你为一个移动app构建一个API)，那么你可以将该文件夹从你的app中删除掉。

##### 编译一个视图
在你可以访问到`res`对象的地方(也就是一个控制器的动作、自定义响应、策略等)，你都可以使用[`res.view`](http://sailsjs.org/documentation/reference/res/res.view.html)来编译你的视图然后发送结果HTML页面给用户。

<<<<<<< HEAD
你也可以直接在`routes.js`文件中路由你的视图文件。只需要指示你的视图文件在`views/`目录下的相对路径，如下所示：
=======
If you prefer to use a different view engine, there are a multitude of options.  Sails supports all of the view engines compatible with [Express](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md) via [Consolidate](https://github.com/visionmedia/consolidate.js/).

Views are defined in your app's [`views/`](http://sailsjs.com/documentation/anatomy/myApp/views) folder by default, but like all of the default paths in Sails, they are [configurable](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md).  If you don't need to serve dynamic HTML pages at all (say, if you're building an API for a mobile app), you can remove the directory from your app.


##### Compiling a view

Anywhere you can access the `res` object (i.e. a controller action, custom response, or policy), you can use [`res.view`](http://sailsjs.com/documentation/reference/res/res.view.html) to compile one of your views, then send the resulting HTML down to the user.

You can also hook up a view directly to a route in your `routes.js` file.  Just indicate the relative path to the view from your app's `views/` directory.  For example:
>>>>>>> upstream/master

```javascript
{
  'get /': {
    view: 'homepage'
  },
  'get /signup': {
    view: 'signupFlow/basicInfo'
  },
  'get /signup/password': {
    view: 'signupFlow/chooseAPassword'
  },
  // and so on.
}
```

##### 那么单页面的app又该如何？
如果你为浏览器构建一个web应用的话，你的导航的全部或者部分可能发生在客户端，也就是说用户在导航的时候不是每次都让浏览器去获取新的HTML页面，而是客户端代码预取一些标记模板然后在用户的浏览器中渲染而不需要直接再次访问服务器。

在这种情况下，你有一些选项来引导单页面app：

+ 使用单一视图，比如`views/publicSite.ejs`，优点：
    + 你可以在Sails中使用模板引擎来直接从服务器传递数据给那些将在客户端被渲染的HTML。这是一个简单的方式去获得信息比如用户数据到客户端的JS中，不需要从客户端发送AJAX或WebSockets请求。

+ 在你的Assets文件夹下使用一个单一的HTML页面，比如`assets/index.html`，优点：
    + 虽然你不能用这种方法直接传递服务器端的数据给用户，但是这种办法允许你将来分离你的应用中的客户端和服务器端部分。
    + 任何在你的Assets文件夹下的任何东西都可以移动到静态的CDN上，这样允许你可以利用提供商在地理上分布的数据中心，让你的数据离用户更近。


<docmeta name="displayName" value="Views">
