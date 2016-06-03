# 钩子技术规范
## 概述
每一个钩子都是使用一个带有单参数的JavaScript函数实现--该参数是对`sails`实例的引用--并返回一个带有一个或者多个关键词的对象，该对象稍后会在这文档中讲解。所以，最基本的钩子应该是像这样的：

```
module.exports = function myBasicHook(sails) {
   return {};
}
```

虽然看着简单，但是它是可以工作的。

每一个钩子应该保存在它自己的带有文件名为`index.js`的文件夹下。这个文件夹名字应该是可以唯一标识钩子，该文件夹可以包含任何数量的额外文件和子文件夹。扩展一下刚才的例子，如果你保存刚才带有`myBasicHook`的文件为`index.js`并放在文件夹`api/hooks/my-basic-hook`，然后使用命令`sails lift --verbose`启动你的app，你将会看到下面的输出：

 `verbose: my-basic-hook hook loaded successfully.`

## 钩子特性
在你的钩子中下面的特性在实现的时候可用。所有的特性都是可选地，并且可以通过添加它们到对象中并返回你的钩子函数来实现。

* [.defaults](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/defaults.html)
* [.configure()](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/configure.html)
* [.initialize()](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/initialize.html)
* [.routes](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/routes.html)


## 自定义的钩子数据和函数
从主钩子函数返回的任何添加到对象的关键词将会在`sails.hooks[<hook name>]`对象中被提供。这就是自定义钩子功能如何提供给终端用户的办法。任何你想保留的私有数据和函数给钩子的都可以被添加到返回的对象*outside*：

 ```
// File api/hooks/myhook/index.js
module.exports = function myHook(sails) {

   // This var will be private
   var foo = 'bar';

   // This var will be public
   this.abc = 123;

   return {

      // This function will be public
      sayHi: function (name) {
         console.log(greet(name));
      }

   };

   // This function will be private
   function greet (name) {
      return "Hi, " + name + "!";
   }

};
```


上面的公共变量和函数可以分别通过`sails.hooks.myhook.abc`和`sails.hooks.myhook.sayHi`来调用。

<docmeta name="displayName" value="Hook Specification">
<docmeta name="stabilityIndex" value="3">
