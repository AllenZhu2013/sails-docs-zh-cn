# Policies
### 概述
在Sails中对于认证和访问控制来说Policies是一个万能的工具--它们让你以一个比较细的粒度来允许或者拒绝访问你的控制器。比如，如果你正在构建Dropbox，在让用户上传一个文件到文件夹之前，你可能会检查它是否`isAuthenticated`，然后确保它`canWrite`，最后你想要检查它上传的文件夹是否还有`hasEnoughSpace`。

策略可以用于任何事情：HTTP BasicAuth，第三方单点登录，OAuth 2.0或者你自己定义的认证方案。

> 注意：策略**只能**用于控制器的动作，不能用于视图。如果你在routes.js中定义一条路由直接指向一个视图，那么将没有任何策略执行。为了确保策略能够执行，你应该定义一个控制器的动作来显示你的视图并且将你的路由执行该动作。

<<<<<<< HEAD
### 写你的第一个策略
策略的文件都是位于`api/policies`文件夹下。每一个策略文件都应该包含一个函数。
=======
> NOTE: policies apply **only** to controller actions, not to views.  If you define a route in your [routes.js config file](http://sailsjs.com/docs/reference/configuration/sails-config-routes) that points directly to a view, no policies will be applied to it.  To make sure policies are applied, you can instead define a controller action which displays your view, and point your route to that action. &nbsp;
>>>>>>> upstream/master

其实归根到底，策略实际上只是运行在你的控制器之前的Connect/Express中间件函数。你可以按照你喜欢的将它们串起来--实际上它们本来就是被设计成用这种方法来使用。理想情况下，每个中间件函数应该只检查一件事情。

比如，我们上面提到的`canWrite`策略应该是这样子的：

```javascript
// policies/canWrite.js
module.exports = function canWrite (req, res, next) {
  var targetFolderId = req.param('id');

  // If the requesting user is not logged in, then they are _never_ allowed to write.
  // No reason to continue-- we can go ahead and bail out now.
  if (!req.session.me) {
    return res.redirect('/login');
  }

  // Check the database to see if a permission record exists which matches both the
  // target folder id, the appropriate "type", and the id of the logged-in user.
  Permission.findOne({
    folder: targetFolderId,
    user: req.session.me,
    type: 'write'
  })
  .exec(function (err, permission) {

    // Unexpected error occurred-- use the app's default error (500) handler.
    //
    // > We do this because this should never happen, and if it does, it means there
    // > is probably something wrong with our database, and we want to know about it!)
    if (err) { return res.serverError(err); }

    // No "write" permission record exists linking this user to this folder.
    // Maybe they got removed from it?  Or maybe they never had permission in the first place...
    if (!permission) {
      return res.redirect('/login');
    }

    // If we made it all the way down here, looks like everything's ok, so we'll let the user through.
    // (from here, the next policy or the controller action will run)
    return next();

  });
};
```

### 使用你的策略保护你的控制器
Sails有内建的ACL，放置在`config/policies.js`文件中。这个文件用于映射你的策略到你的控制器中。

<<<<<<< HEAD
这个文件只是说明性的，所以意味着它描述了你的app的权限应该是什么样子的，而不是他们应该怎么工作。这让开发者容易投入并了解这是怎么回事，加上它让你的app更加灵活当你的要求随着时间的过去不可避免地改变。
=======
### Protecting Controllers with Policies

Sails has a built in ACL (access control list) located in `config/policies.js`.  This file is used to map policies to your controllers.

This file is  *declarative*, meaning it describes *what* the permissions for your app should look like, not *how* they should work.  This makes it easier for new developers to jump in and understand what's going on, plus it makes your app more flexible as your requirements inevitably change over time.
>>>>>>> upstream/master

你的`config/policies.js`应该导出一个JS对象，该对象的关键词是控制器的名字(对于`‘*’`的是全局策略)，其值则是一个映射动作的名字到一个或多个策略的对象，参考下面的例子。

##### 应用一条策略到一个专用的控制器动作：

```js
{
  ProfileController: {
      // Apply the 'isLoggedIn' policy to the 'edit' action of 'ProfileController'
      edit: 'isLoggedIn'
      // Apply the 'isAdmin' AND 'isLoggedIn' policies, in that order, to the 'create' action
      create: ['isAdmin', 'isLoggedIn']
  }
}
```

##### 应用一条策略到整个控制器中：


```js
{
  ProfileController: {
    // Apply 'isLoggedIn' by default to all actions that are NOT specified below
    '*': 'isLoggedIn',
    // If an action is explicitly listed, its policy list will override the default list.
    // So, we have to list 'isLoggedIn' again for the 'edit' action if we want it to be applied.
    edit: ['isAdmin', 'isLoggedIn']
  }
}
```

> 注意：默认的策略映射不是串联或者具有涓滴效应的。专用的控制器动作映射应该覆盖默认的映射。


##### 应用一条策略到所有的不明确映射的动作：

```js
{
  // Apply 'isLoggedIn' to all actions by default
  '*': 'isLoggedIn',
  ProfileController: {
      // Apply 'isAdmin' to the 'foo' action.  'isLoggedIn' will NOT be applied!
      'foo': 'isAdmin'
  }
}
```

> 记住默认的策略不会应用到任何给出明确映射的控制器或者动作上。

### 内建策略
Sails提供两组内建的策略可以全局使用或者专用于控制器或者动作：

+ `true`：公共访问权限(允许任何人访问控制器或者动作)
+ `false`：**不允许**访问(**不允许任何人**访问控制器和动作)

`'*': true`是所有控制器和动作的默认策略。在产品模式下，这是它为`false`是最好的主意，这是为了阻止访问那些你可能无意暴露出来的逻辑。

<<<<<<< HEAD
=======
##### Using policies with blueprint actions

Sails' built-in [blueprint API](http://sailsjs.com/documentation/concepts/blueprints) is implemented using regular Sails controller actions.  The only difference is that blueprint actions are implicit.

To apply your policies to blueprint actions, set up your policy mappings just like we did in the example above, but pointed at name of the relevant implicit [blueprint action](http://sailsjs.com/documentation/concepts/blueprints/blueprint-actions) in your controller.  For example:
```js
{
  UserController: {
    // Apply the 'isLoggedIn' policy to the 'update' action of 'UserController'
    update: 'isLoggedIn'
  }
}
```


### Built-in policies
Sails provides two built-in policies that can be applied globally, or to a specific controller or action.
  + `true`: public access  (allows anyone to get to the mapped controller/action)
  +  `false`: **NO** access (allows **no-one** to access the mapped controller/action)
>>>>>>> upstream/master

##### 添加一些策略到一个控制器中

```javascript
  // in config/policies.js

  // ...
  RabbitController: {

    // Apply the `false` policy as the default for all of RabbitController's actions
    // (`false` prevents all access, which ensures that nothing bad happens to our rabbits)
    '*': false,

    // For the action `nurture`, apply the 'isRabbitMother' policy
    // (this overrides `false` above)
    nurture : 'isRabbitMother',

    // Apply the `isNiceToAnimals` AND `hasRabbitFood` policies
    // before letting any users feed our rabbits
    feed : ['isNiceToAnimals', 'hasRabbitFood']
  }
  // ...
```

上面的`isNiceToAnimals`应该是这样子的(这个文件位于`policies/isNiceToAnimals.js`)

```javascript
module.exports = function isNiceToAnimals (req, res, next) {

  // `req.session` contains a set of data specific to the user making this request.
  // It's kind of like our app's "memory" of the current user.

  // If our user has a history of animal cruelty, not only will we
  // prevent her from going even one step further (`return`),
  // we'll go ahead and redirect her to PETA (`res.redirect`).
  if ( req.session.user.hasHistoryOfAnimalCruelty ) {
    return res.redirect('http://PETA.org');
  }

  // If the user has been seen frowning at puppies, we have to assume that
  // they might end up being mean to them, so we'll
  if ( req.session.user.frownsAtPuppies ) {
    return res.redirect('http://www.dailypuppy.com/');
  }

  // Finally, if the user has a clean record, we'll call the `next()` function
  // to let them through to the next policy or our controller
  next();
};
```

除了保护rabbits之外，这儿还有一些其他策略的使用case：
+ cookie-based认证
+ role-based访问控制
+ 基于MB配额限制文件上传
+ 任何你可以想到的各种认证组合



<docmeta name="displayName" value="Policies">
