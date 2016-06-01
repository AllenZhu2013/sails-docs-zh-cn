# Blueprint Routes
当你使能blueprint并且运行`sails lift`的时候，该web框架将会检查你的控制器、模型、以及配置，这是为了自动[绑定某些路由](http://sailsjs.org/documentation/concepts/Routes)。这些隐性的blueprint路由(有时候我们也称为"隐藏的")允许你的app去响应某些请求而不需要你在`config/routes.js`文件中手工绑定那些路由。默认的，blueprint路由指向了它们对应的blueprint*动作*(参考上一小节的**Blueprint Actions**)，它们的所有代码都是可以对其进行重写的。

在Sails中有下面3种类型的blueprint路由：

+ **RESTful routes**，其路径总是这样的：`/:modelIdentity`或`/:modelIdentity/:id`。这些路由使用HTTP "verb"来决定采取的动作；比如一个`POST`请求到`/user`将会创建一个新的用户，并且一个`DELETE`请求到`/user/123`将会删除一个有主要关键词123的用户。在产品环境下，RESTful routes应该通过[策略](http://sailsjs.org/documentation/concepts/Policies)来保护不受未认证地访问。

+ **Shortcut routes**，这种类型的路由采取的动作将会体现在路径中。比如，`/user/create?name=joe`快捷方式创建一个新的用户，`/user/update/1?name=mike`则会更新用户#1。这些路由只会对`GET`请求回应。Shortcut routes对开发特别便利，但是一般在产品模式下需要禁用掉。

+ **Action routes**，将会自动地为你的自定义的控制器动作创建路由。比如，如果你有一个`FooController.js`文件并带有一个`bar`方法，那么一条`/foo/bar`的路由将会自动为你创建只要blueprint动作路由有使能。不像RESTful and shortcut routes，动作路由*不*要求一个控制器有一个对应的模型文件。

blueprint配置选项请参考[blueprints subsection of the configuration reference](http://sailsjs.org/documentation/reference/sails.config/sails.config.blueprints.html)，其中包括如何使能或禁用不同的blueprint路由类型。

<docmeta name="displayName" value="Blueprint Routes">
