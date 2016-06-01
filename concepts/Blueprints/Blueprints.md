# Blueprints
### 概述
就像其他优秀的框架一样，Sails也是致力于减少你写代码的数量以及你让一个功能性的app运行起来所花费的时间。*Blueprints*是Sails基于你的应用设计快速生成API[路由](http://sailsjs.org/documentation/concepts/routes)和[动作](http://sailsjs.org/documentation/concepts/controllers#?actions)的方法。 

同时，[blueprint routes](http://sailsjs.org/documentation/concepts/blueprints/blueprint-routes)和 [blueprint actions](http://sailsjs.org/documentation/concepts/blueprints/blueprint-actions)构成了**blueprint API**，其内建的逻辑可以在你每次创建一个模型和控制器的时候提供强有力的[RESTful JSON API](http://en.wikipedia.org/wiki/Representational_state_transfer)。

比如如果你在你的工程中创建了一个`User.js`模型和`UserController.js`控制器文件，然后使能blueprints，那么你可以直接地访问`/user/create?name=joe`来创建一个用户，并且访问`/user`来查看你的app中用户数组。所有的这些都是无须多写一行代码！

Blueprints不仅是一个伟大的原型设计，而且它在产品中也是一款非常有用的工具因为它们的特性完全可以被重写、保护、扩展或者禁用。

<docmeta name="displayName" value="Blueprints">
