# 禁用全局变量
在[`sails.config.globals`](http://sailsjs.org/documentation/reference/sails.config/sails.config.globals.html)决定了哪些Sails的全局变量会被暴露出来，可以通过文件[`config/globals.js`](http://sailsjs.org/documentation/anatomy/myApp/config/globals.js.html)方便地修改。

<<<<<<< HEAD
为了禁用所有的全局变量，只需要将下面的设置置为`false`：
=======
Sails determines which globals to expose by looking at [`sails.config.globals`](http://sailsjs.com/documentation/reference/sails.config/sails.config.globals.html), which is conventionallly configured in [`config/globals.js`](http://sailsjs.com/documentation/anatomy/config/globals.js.html).

To disable all global variables, just set the setting to `false`:
>>>>>>> upstream/master

```js
// config/globals.js
module.exports.globals = false;
```

为了禁用一些全局变量，指定成对应的对象，比如：

```js
// config/globals.js
module.exports.globals = {
  _: false,
  async: false,
  models: false,
  services: false
};
```

###注意：

> 请记住在`Sails`没有完全启动之前所有的全局变量都是不可访问的，包括`sails`。换句话说，你不能在一个函数之外使用`sails.models.user`或者`User`(因为这时`sails`还没有加载完毕)。

<!-- not true anymore:
Most of this section of the docs focuses on the methods and properties of `sails`, the singleton object representing your app.
-->

<docmeta name="displayName" value="Disabling Globals">
