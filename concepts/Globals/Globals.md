# Globals
### 概述
为了便利，Sails暴露出一些全局变量。默认的，你的app的[models](http://sailsjs.org/documentation/reference/Models)、[services](http://sailsjs.org/documentation/reference/Services)和全局`sails`对象在全局范围内都是可用的；这意味着你可以在你的后端代码中的任何地方引用它们。(只要Sails[运行起来](https://github.com/balderdashy/sails/tree/master/lib/app))

Sails的核心没有依赖于这些全局变量，所以Sails提供的每个全局变量都可以在`sails.config.globals`中禁用掉(更方便地可以在`config/globals.js`中配置)。

### App对象(Sails)
在大部分的情况下，你想要让`Sails`对象全局可用-这让你的代码变得更整洁。然而，如果你需要禁用掉所有的全局变量，包括`sails`，你可以再请求对象(`req`)中访问到`sails`。

### Models和Services
你的app的[models](http://sailsjs.org/documentation/reference/Models)和[services](http://sailsjs.org/documentation/reference/Services)暴露为全局变量通过使用它们的`globalId`。比如，定义在文件`api/models/Foo.js`的models可以通过`Foo`全局访问到，定义在`api/services/Baz.js`文件中的services `Baz`可以全局访问到。

### Async (async) and Lodash (_)
Sails同时也通过`_`暴露[lodash](http://lodash.com/)的实例，通过[async](https://github.com/caolan/async)暴露`async`的实例。默认提供这些常用的功能所以你不需要自己安装它们到每个工程。就像Sails的其他全局变量一样，它们都是可以被禁用的。



<docmeta name="displayName" value="Globals">
