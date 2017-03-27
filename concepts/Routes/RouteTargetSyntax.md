# 自定义路由
### 概述
Sails允许在你的**config/routes.js**文件中明确用不同的方法路由URL。每一条路由都包括**地址**和**目标**，如下：

```
'GET /foo/bar': 'FooController.bar'
^^^address^^^^  ^^^^^^target^^^^^^^
```

### 路由地址
路由地址指示URL应该匹配什么，以便应用目标定义的操作以及选项。一条路由地址包括可选的verb和一个必选的路径：

```
'POST  /foo/bar'
^verb^ ^^path^^
```

<<<<<<< HEAD
如果没有verb指定，目标将会被应用到匹配路径的任何请求，忽略使用的HTTP方法(**GET**/**POST**/**PUT**等)。注意路径中的初始字符`/`--所有的路径都应该以该字符作为起始字符否则会出错。
=======
If no verb is specified, the route will match any CRUD method (**GET**, **PUT**, **POST**, **DELETE** or **PATCH**).  If `ALL` is specified as the verb, the route will match _any_ method.

Note the initial `/` in the path--all paths should start with one in order to work properly.

#### Wildcards and dynamic parameters
>>>>>>> upstream/master

#### 通配符和动态参数
为了像**foo/bar**一样指定一个静态路径，你可以使用`*`作为通配符：

```
'/*'
```

将会匹配到所有的路径，如：

```
'/user/foo/*'
```

该规则将会匹配所有的以/user/foo开始的路径。

<<<<<<< HEAD
> **注意**：当使用带有通配符的路由，比如`'/*'`,这也会匹配到请求静态assets(也就是`/js/dependencies/sails.io.js`)并覆盖它们。为了避免这个，考虑使用[下面描述](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options)的`skipAssets`选项。
=======
> **Note:** When using a route with a wildcard, such as `'/*'`, be aware that this will also match requests to static assets (i.e. `/js/dependencies/sails.io.js`) and override them. To prevent this, consider using the `skipAssets` option [described below](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options).
>>>>>>> upstream/master

你可以通过使用`:paramName`通配符语法而不是使用`*`来匹配名字参数中的某一部分:

```
'/user/foo/bar/:name'
```

<<<<<<< HEAD
将会匹配相同的URL如下：
=======
Will match _almost_ the same URLs as:
>>>>>>> upstream/master

```
'/user/foo/bar/*'
```

<<<<<<< HEAD
但是也会分别提供路由的通配符部分的值到路由操作`req.param('name')`和`req.param('age')`。
=======
but will provide the value of the dynamic portions of the route URL to the route handler as parameter values.  In this case: `req.param('name')`.

> Note that the wildcard (`*`) syntax matches slashes, where the URL pattern variable (`:`) syntax does not.  So in the example above, given the route address `GET /user/foo/bar/*`, incoming requests with URLs like `/user/foo/bar/baz/bing/bong/bang` would match (whereas if you used the `:name` syntax, the same URL would not match.)
>>>>>>> upstream/master


#### 地址中的正则表达式
除了通配符语法，你可能也使用正则表达式来定义一条路由匹配的URLs。使用正则表达式来定义地址的语法如下：

`"r|<regular expression string>|<comma-delimited list of param names>"`

字母“r”后面跟着一个管道符`|`,一条正则表达式没有分隔符，另一个管道符后面应该跟着一组参数名字--应该被映射到加括号的组中。比如：

`"r|^/\\d+/(\\w+)/(\\w+)$|foo,bar": "MessageController.myaction"`

将会匹配`/123/abc/def`，运行`MessageController`中的`myaction`的动作并提供`abc`和`def`的值分别作为`req.param('foo')`和`req.param('bar')`的结果。

注意双反斜杠`\\d`和`\\w`是为了转义之用。

#### 关于路由顺序
当在你的地址中使用通配符或者正则表达式，请注意在你的**config/routes.js**中的路由顺序。URL将会匹配从上到下的匹配地址列表。如果你在这个规则中有两条配置：

    '/user': 'UserController.doSomething',
    '/*'   : 'CatchallController.doSomethingElse'

<<<<<<< HEAD
那么到`/user`的请求将不会匹配到第二条配置除非第一条配置的操作在它的代码中调用`next()`，这种配置是不鼓励的(只有[policies](http://sailsjs.org/documentation/concepts/Policies)才应该调用`next()`)。除非你使用的东西非常先进，那么可以安全地假设每一条请求至多被一条路由处理在你的**config/routes.js**文件。
=======
then a request to `/user` will not be matched by the second configuration unless the first configuration's handler calls `next()` in its code, which is discouraged (only [policies](http://sailsjs.com/documentation/concepts/Policies) should call `next()`).  Unless you're doing something very advanced, it is safe to assume that every request will be handled by at most one route in your **config/routes.js** file.

### Route Target
>>>>>>> upstream/master


### 路由目标
自定义的请求中的地址那一部分是指示URL应该匹配的路由，那么目标那一部分是指示Sails应该在匹配之后做些什么操作。一条目标可以使用多种不同格式中的一个。在一些情况下你也许想要将多条目标串成一个单一的地址通过将它们放到一个数组中，但是大部分情况是每一条地址只有一条目标。目标的不同种类型将在下面讨论，并且也会讨论应用到它们中的各种选项。

<<<<<<< HEAD
#### 控制器/动作的目标语法
目标的最常见的类型是绑定一条路由到一个自定义的[controller action](http://sailsjs.org/documentation/concepts/Controllers?q=actions)，下面的4条路由都是等价的：
=======
The most common type of target is one which binds a route to a custom [controller action](http://sailsjs.com/documentation/concepts/Controllers?q=actions).  The following four routes are equivalent:
>>>>>>> upstream/master

```
'GET /foo/go': 'FooController.myGoAction',
'GET /foo/go': 'Foo.myGoAction',
'GET /foo/go': {controller: "Foo", action: "myGoAction"},
'GET /foo/go': {controller: "FooController", action:"myGoAction"},

```

每一条映射`GET /foo/go`到在**api/controllers/FooController.js**控制器中的`myGoAction`动作。如果没有这样的动作或者控制器存在，Sails将会输出一个错误信息并忽略该路由。否则，无论什么时候一条**GET**请求到**/foo/go**，那动作的代码都会执行。

在这个语法中的控制器和动作的名字是区分大小写的。

<<<<<<< HEAD
注意[blueprint API](http://sailsjs.org/documentation/reference/blueprint-api)默认地添加一些动作到你的控制器中(比如"find", "create", "update" and "delete")，所有的这些动作都是在路由中可用的：
=======
Note that the [blueprint API](http://sailsjs.com/documentation/reference/blueprint-api) adds several actions to your controllers by default (like "find", "create", "update" and "delete"), all of which are available for routing:
>>>>>>> upstream/master

 ```
'GET /foo/go': 'UserController.find'
```

<<<<<<< HEAD
假设你有**api/controllers/UserController.js**文件和一个**api/models/User.js**文件，在浏览器中浏览**/foo/go**将会使用上述的配置，运行默认的“find*” blueprint动作来显示所有的`User`模型的列表。如果你有一个[自定义的动作](http://sailsjs.org/documentation/concepts/Controllers?q=actions)叫做`find`在UserController，该动作会替换掉默认的动作而被执行。

#### 视图的目标语法
另外一个常见的目标是绑定一条路由到一个[视图](http://sailsjs.org/documentation/concepts/Views)中。这个语法非常容易，直接指向**视图**对应的路径，不需要文件的后缀名：
=======
Assuming that your have a **api/controllers/UserController.js** file and a **api/models/User.js** file, browsing to **/foo/go** in a browser will, using the above config, run the default "find* blueprint action which displays a list of all `User` models.  If you have a [custom action](http://sailsjs.com/documentation/concepts/Controllers?q=actions) named `find` in UserController, that action will be run instead.


#### View target syntax

Another common target is one that binds a route to a [view](http://sailsjs.com/documentation/concepts/Views).  This is particularly useful for binding static views to a custom URL, and it's how the default homepage for new projects is set up out of the box.

The syntax for view targets is simple: it is just the path to the view file, without the file extension (e.g. `.ejs`) and relative to the **views/** folder :
>>>>>>> upstream/master

```
'GET /team': {view: 'brochure/about'}
```

<<<<<<< HEAD
这个将映射`GET /home`到存储在**views/home/index.ejs**(假设使用EJS[模板引擎](http://sailsjs.org/documentation/concepts/Views/ViewEngines.html))的视图。只要对应的视图存在，那么**GET**请求到**/home**的将会显示。
=======
This tells Sails to handle `GET` requests to `/team` by serving the view template located at `views/brochure/about.ejs` (assuming the default EJS [template engine](http://sailsjs.com/documentation/concepts/Views/ViewEngines.html) is used).  As long as that view file exists, a **GET** request to  **/home** will display it. For consistency with Express/consolidate, if the specified relative path does not match a view file, then Sails will look for a sub-folder with the same name (e.g. `pages/brochure`) and serve the "index" view in that sub-folder (e.g. `pages/brochure/index.ejs`) if one exists.

> Note that since this route is bound directly to the view, none of your configured policies will be applied.  If you need to configure a policy, use `res.view()` from a controller action.  See [this StackOverflow question](http://stackoverflow.com/questions/21303217/sailsjs-policy-based-route-with-a-view/21340313#21340313) for more background information.
>>>>>>> upstream/master

> 注意这条路由将会直接被绑定到视图中，没有任何策略应用到这个例子中，参考 [this StackOverflow question](http://stackoverflow.com/questions/21303217/sailsjs-policy-based-route-with-a-view/21340313#21340313)。

<<<<<<< HEAD
#### Blueprint 的目标语法
在一些情况下你可能需要映射一个非标准的地址到一个Sails的Blueprint动作。比如如果你有一个控制器和动作分别叫做**UserController**和**User**，那么Sails将会自动映射**GET /user**到“find” blueprint 动作并返回一个User记录的列表。如果你想要映射一个不同地址到该动作上，你可以使用下面的语法：
=======
In some cases you may want to map a non-standard address to one of the Sails [blueprint actions](http://sailsjs.com/documentation/reference/blueprint-api?q=blueprint-actions).  For example, if you have a controller and model named **UserController** and **User** respectively, then Sails will automatically map **GET /user** to the "find" blueprint action which returns a list of User records.  If you'd like to map a different address to that action, you can use the blueprint target syntax:
>>>>>>> upstream/master

```
'GET /findAllUsers': {model: 'user', blueprint: 'find'},
'GET /user/findAll': {blueprint: 'find'}
```

注意在这个配置里，`model`和`blueprint`的属性都要被设置，但是只有第二种也就是只有`blueprint`会被使用。在第二种配置中，删去`model`属性会导致Sails去检查地址并猜测模型是`User`。你可以覆盖掉这个通过明确地设置`Model`为别的：

```
'GET /user/findAll': {blueprint: 'find', model: 'pet'}
```

即使你很少想要这样做，但是仍然建议你不要这样做，因为它会让你的app的API变得混乱和让人混淆。

如果你指定一个没有扩展的model或者blueprint在你的配置中，Sails将会输出一个错误并忽略掉这个路由。

你也可以使用这个语法去映射一条路由到默认的blueprint动作中的一个即使你在你的控制器中已经重写那动作。

#### 重定向语法
你可以重定向一条地址到另一条--既可以在你的服务器上也可以是完全地在别的服务器上--只需指定一条重定向的URL字符串如下：

```
'/alias' : '/some/other/url'
```

当在你的Sails app中重定向的时候小心重定向死循环！

<<<<<<< HEAD
注意的是当重定向的时候，原始请求的HTTP方法(和其他额外的头部参数)将会丢失，并且请求将会被传输到一条简单的**GET**请求。在上述的实例中，一条**POST**请求到**/alias**的将会得到一条**GET**请求到**/some/other/route**的结果。这就有点依赖于浏览器的行为了，但是建议你不要期望请求方法以及数据从一条重定向中。

#### 响应的目标语法
你可以直接映射一个地址到一条默认的或者自定义的响应通过使用下面的语法：
=======
Note that when redirecting, the HTTP method of the original request (and any extra headers / parameters) will likely be lost, and the request will be transformed to a simple **GET** request.  In the above example, a **POST** request to **/alias** will result in a **GET** request to **/some/other/route**.  This is somewhat browser-dependent behavior, but it is recommended that you don't expect request methods and other data to survive a redirect.

#### Response target syntax
You can map an address directly to a default or custom [response](http://sailsjs.com/documentation/concepts/Custom-Responses) using this syntax:
>>>>>>> upstream/master

```
'/foo': {response: 'notFound'}
```

在你的**api/responses**文件夹中简单地指定响应文件的名字，不需要**.js**后缀名。在这条语法中响应名字是区分大小写的。如果你尝试绑定一条路由到一条没有扩展的响应，Sails将会输出错误并忽略这条路由。

<<<<<<< HEAD
#### 策略的目标语法
在大部分情况下，你想要使用[**config/policies.js**](http://sailsjs.org/documentation/reference/sails.config/sails.config.policies.html)配置文件来应用[policies](http://sailsjs.org/documentation/concepts/Policies)到你的控制器的动作中。然而也有些时候当你想要直接应用一条策略到一条自定义的路由：尤其当你想要使用[view](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=view-target-syntax)或者[blueprint]((http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=blueprint-target-syntax)目标语法的时候。下面的策略目标语法是：
=======
#### Policy target syntax

In most cases, you will want to apply [policies](http://sailsjs.com/documentation/concepts/Policies) to your controller actions using the [**config/policies.js**](http://sailsjs.com/documentation/reference/sails.config/sails.config.policies.html) config file.  However, there are some times when you will want to apply a policy directly to a custom route: particularly when you are using the [view](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=view-target-syntax) or [blueprint]((http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=blueprint-target-syntax) target syntax.  The policy target syntax is:
>>>>>>> upstream/master

```js
'/foo': { policy: 'myPolicy' }
```

然而，你总是想要串联策略到至少一种类型的目标中，使用这个数组：

```js
'/foo': [
  { policy: 'myPolicy' },
  { controller: 'user', action: 'find' }
]
```

这个将会应用**myPolicy**策略到你的路由并且，如果通过的话，将会继续为**User**模型运行**find**的blueprint。

<<<<<<< HEAD
=======
#### Function target syntax

For quick-and-dirty jobs (useful for quick tests), you can assign a route directly to a function:
```js
'/foo': function(req, res) { return res.send('hello!'); }
```

You can also combine this syntax with others using an array, allowing you to define quick, inline middleware:

```js
'/foo': [
  function(req, res, next) {
    sails.log('Quick and dirty test:', req.allParams());
    return next();
  },
  { controller: 'user', action: 'find' }
]
```

You can also use a dictionary with an `fn` key to assign a function.  This allows you to also specify [other route target  options](http://sailsjs.com/documentation/concepts/routes/custom-routes#?route-target-options) at the same time:
```js
'GET /*': {
  fn: function(req, res) {
    return res.send('hello!');
  },
  skipAssets: true
}
```

> Best practice is to use the function syntax only for temporary routes, since it goes against the structural conventions that make Sails useful!  (Plus, the less cluttered your routes.js file, the better.)

### Route target options
>>>>>>> upstream/master

### 路由目标选项
除了上面讨论的各种路由目标语法提到的选项之外，其他你会添加到路由目标对象中的属性将会被传递到路由操作器在req.options对象中。这里有一些保留的属性可以用于影响路由操作性的行为。列表如下：

| Property    | Applicable Target Types       | Data Type | Details |
|-------------|:----------:|-----------|-----------|
|`skipAssets`|all|((boolean))|Set to `true` if you *don't* want the route to match URLs with dots in them (e.g. **myImage.jpg**).  This will keep your routes with [wildcard notation](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=wildcards-and-dynamic-parameters) from matching URLs of static assets.  Useful when creating [URL slugs](/#/documentation/concepts/Routes/URLSlugs.html).|
|`skipRegex`|all|((regexp))|If skipping every URL containing a dot is too permissive, or you need a route's handler to be skipped based on different criteria entirely, you can use `skipRegex`.  This option allows you to specify a regular expression or array of regular expressions to match the request URL against; if any of the matches are successful, the handler is skipped.  Note that unlike the syntax for binding a handler with a regular expression, `skipRegex` expects *actual [RegExp objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)*, not strings.|
|`locals`|[controller](http://sailsjs.com/#!/documentation/concepts/Routes/RouteTargetSyntax.html?q=controller-%2F-action-target-syntax), [view](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=view-target-syntax), [blueprint](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=blueprint-target-syntax), [response](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=response-target-syntax)|((dictionary))|Sets default [local variables](http://sailsjs.com/documentation/reference/res/res.view.html?q=arguments) to pass to any view that is rendered while handling the request.|
|`cors`|all|((dictionary)) or ((boolean)) or ((string))|Specifies how to handle requests for this route from a different origin.  See the [main CORS documentation](http://sailsjs.com/documentation/concepts/CORS) for more info.|
|`populate`|[blueprint](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=blueprint-target-syntax)|((boolean))|Indicates whether the results in a "find" or "findOne" blueprint action should have associated model fields [populated](http://sailsjs.com/documentation/reference/waterline/populated-values).  Defaults to the value set in [**config/blueprints.js**](http://sailsjs.com/documentation/reference/sails.config/sails.config.blueprints.html).
|`skip`, `limit`, `sort`, `where`|[blueprint](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=blueprint-target-syntax)|((dictionary))|Set criteria for "find" blueprint.  See the [queries reference](https://github.com/balderdashy/sails-docs/blob/master/reference/waterline/queries/queries.md) for more info.


<docmeta name="displayName" value="Custom Routes">
