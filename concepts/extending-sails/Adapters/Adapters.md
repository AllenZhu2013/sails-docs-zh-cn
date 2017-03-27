<<<<<<< HEAD
# 适配器
### 状态
##### 稳定性：[3](http://nodejs.org/api/documentation.html#documentation_stability_index)级-稳定
这个API已经证明是满足要求的，但是底层代码的清理可能会导致轻微的改变。向后兼容是必须的。

### 什么是适配器？
适配器是将实现具体的函数抽象出来然后提供一个**接口**给外部调用。这允许我们可以保证跨多种模型、开发者、app甚至是公司去方便地使用这些类型，让app的代码变得更加可维护性，更加有效率，更加可靠。适配器用于集成数据库，开放的API，内部的或者专用的web服务乃至硬件。

### 在一个适配器里我们能够做哪类事情？
适配器主要关注提供模型上下文化的CRUD方法。CRUD代表创建、读取、更新和删除。在Sails或者Waterline中，我们称这些方法为`create()`,`find()`,`update()`和`destroy()`。

比如，一个`MySQLAdapter`实现一个`create()`的方法，该方法在内部使用专用的表名称去调用一个MySQL数据库并连接然后运行一个`INSERT...`SQL查询。

实际上，你的适配器可以做任何你想做的事情--你写的任何方法都会暴露在原始连接对象中和任何会使用它们的模型中。
=======
# Adapters

### What is an adapter?

In Sails and Waterline, database adapters (often simply called "adapters", for short) allow the models in your Sails app to communicate with your database(s). In other words, when your code in a controller action or helper calls a model method (e.g. `User.find()`), what happens next is determined by the adapter configured for that model (e.g. `User`).

An adapter is defined as a dictionary (aka JavaScript object, like `{}`) with methods like `find`, `create`, etc.  Based on which methods it implements, and the completeness with which they are implemented, adapters are said to implement one or more **interface layers**.  Each interface layer implies a contract to implement certain functionality.  This allows Sails and Waterline to guarantee conventional usage patterns across multiple models, developers, apps, and even companies, making app code more maintainable, efficient, and reliable.  

> In previous versions of Sails, adapters were sometimes used for other purposes, like communicating with certain kinds of RESTful web APIs, internal/proprietary web services, or even hardware.  But _truly_ RESTful APIs are very rare, and so, in most cases, writing a database adapter to integrate with a _non-database API_ can be limiting.  Luckily, there is now a [more straightforward way](http://node-machine.org/machinepacks) to build these types of integrations.
>>>>>>> upstream/master

##### 类方法
下面提到的`class methods`指的是静态的或者面向集合的，在模型本身中函数可用的。比如：`User.create()`或`Menu.update()`。为了添加自定义的类方法到你的模型中(除了在适配器中提供它实现了什么)，定义它们作为顶层的关键词或者函数对在model对象中。

##### 实例方法
另一方面`instance methods`(也是众所周知的对象、模型或者方法)指的是在独立的结果模型中那些可用的方法，比如：`User.findOne(7).done(function (err, user) { user.someInstanceMethod(); })`。为了添加自定义的方法到你的模型中(除了在适配器中提供它实现了什么)，定义它们作为顶层的关键词或者函数对在模型定义的地方中的`attributes`对象。

<<<<<<< HEAD
##### DDL和自动迁移
`DDL`表示的是数据定义语言，它是面向模式的数据库的基本特征。在Sails中，自动迁移支持立即完成。因为对于大部分常见的支持`alter()`的SQL的适配器，所以它们也支持自动地模式迁移。在你自己的适配器中，如果你写了`alert()`方法，那么相同的行为将会生效。这个特征是可以使用`migrate`属性来进行配置，该属性可以设置成`safe`(不会修改架构、期限)、`drop`(每次app启动的时候重新生成表)、或者`alter`(默认的配置--合并app中的模型的纲要到目前数据库存在的数据中)。
=======
Adapters are mainly focused on providing model-contextualized CRUD methods.  CRUD stands for create, read, update, and delete.  In Sails/Waterline, we call these methods `create()`, `find()`, `update()`, and `destroy()`.

For example, a `MySQLAdapter` implements a `create()` method which, internally, calls out to a MySQL database using the specified table name and connection information and runs an `INSERT ...` SQL query.


### Next steps
>>>>>>> upstream/master

Read about [available adapters](http://sailsjs.com/docs/concepts/extending-sails/adapters/available-adapters), or how to make your own [custom adapter](http://sailsjs.com/docs/concepts/extending-sails/adapters/custom-adapters).


<docmeta name="displayName" value="Adapters">
<docmeta name="stabilityIndex" value="3">
