# Blueprint Actions
Blueprint actions(不要和blueprint action *routes*相混淆)是设计成与你的所有有相同名称的模型的控制器协同工作的通用动作(比如`ParrotController`将会需要一个`Parrot`的模型)。你可以将它们想象成你的应用中默认的行为。比如，如果你有一个`User.js`模型并且有一个空的`UserController.js`控制器，那么`find`,`create`,`update`,`destroy`, `populate`,`add`和`remove`的动作将会隐式的存在，不需要你去实现它们。

默认的blueprint RESTful routes和shortcut routes被绑定到它们对应的blueprint 动作中。但是任何blueprint动作都是可以通过在对应控制器的文件(比如`ParrotController.find`)中创建一个自定义的动作来为某一个特定的控制器重写.另外，你可以通过创建你自己定义的blueprint动作来在*你的app中的任何地方*重写blueprint动作(比如 `api/blueprints/create.js`)。

当前版本的Sails有以下blueprint动作：

<<<<<<< HEAD
+ [find](http://sailsjs.org/documentation/reference/blueprint-api/Find.html)
+ [findOne](http://sailsjs.org/documentation/reference/blueprint-api/FindOne.html)
+ [create](http://sailsjs.org/documentation/reference/blueprint-api/Create.html)
+ [update](http://sailsjs.org/documentation/reference/blueprint-api/Update.html)
+ [destroy](http://sailsjs.org/documentation/reference/blueprint-api/Destroy.html)
+ [populate](http://sailsjs.org/documentation/reference/blueprint-api/Populate.html)
+ [add](http://sailsjs.org/documentation/reference/blueprint-api/Add.html)
+ [remove](http://sailsjs.org/documentation/reference/blueprint-api/Remove.html)

关于blueprint更多信息，包括如何禁用它以及重写它们，请参考[Blueprint API参考文档](http://sailsjs.org/documentation/reference/blueprint-api)。
=======
+ [find](http://sailsjs.com/documentation/reference/blueprint-api/Find)
+ [findOne](http://sailsjs.com/documentation/reference/blueprint-api/FindOne)
+ [create](http://sailsjs.com/documentation/reference/blueprint-api/create)
+ [update](http://sailsjs.com/documentation/reference/blueprint-api/Update)
+ [destroy](http://sailsjs.com/documentation/reference/blueprint-api/Destroy)
+ [populate](http://sailsjs.com/documentation/reference/blueprint-api/Populate)
+ [add](http://sailsjs.com/documentation/reference/blueprint-api/Add)
+ [remove](http://sailsjs.com/documentation/reference/blueprint-api/Remove)

##### Overriding blueprint actions

You may also override any of the actions for a model by naming your function method the same name as the blueprint action.

```javascript
module.exports = {
  findOne: function (req, res) {
    return res.json(403, 'Single model lookup is denied.');
  }
}

```
For more information about blueprints, including how to disable and override them, see the [Blueprint API reference](http://sailsjs.com/documentation/reference/blueprint-api)

>>>>>>> upstream/master

<docmeta name="displayName" value="Blueprint Actions">
