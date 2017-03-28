# 钩子
## 状态
> ##### 稳定性：[3](http://nodejs.org/api/documentation.html#documentation_stability_index)-稳定

## 什么是钩子？
钩子就是一个能够添加功能到Sails内核的Node模块。[钩子的技术规范](http://sailsjs.org/documentation/concepts/extending-sails/hooks/hook-specification)定义了需求--一个模块需要符合Sails，能够导入它的代码并且让新的功能可用。因为它们是与内核分开保存的，所以钩子允许Sails代码可以在apps和开发者之间共享而不需要修改框架。

## 钩子与Services的关联与区别
钩子与Sails的[serivces](http://sailsjs.org/documentation/concepts/Services)共享一些公共的特性。它们都允许开发者存储公共使用的代码在一个位置，它们也都可以让一个新的方法在一个Sails app中全局可用。然而，它们还是有一些关键的不同：

+ Services不能单独保存在一个app中。但是一些类型的钩子也许可以绑定到一个单独的app(参考 [Project Hooks](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html))，其他类型也可以在一个Sails app中独立开发和使用`npm install`安装。
+ 钩子又自己的初始化系统。这允许它们可以在Sails开始运行的时候自己配置，也就是说更加具有动态性。
+ 钩子可以在Sails启动之前添加新的[routes](http://sailsjs.org/documentation/concepts/Routes)到一个Sails app中去。

Services对于那些可以在多个[控制器](http://sailsjs.org/documentation/concepts/Controllers)和[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)共享的代码仍然是一个好的选择，但是：

+ 不大可能在另外一个app中重用代码
+ 在不同的环境不需要表现得不一样(比如开发环境和产品环境)

对于所有其它可重用的代码，钩子是一个很好的选择。

## 钩子的类型
在Sails中有下面三种类型的钩子可用

1. **Core hooks**。这类的钩子提供了一个Sails app绝大部分的公共的必不可少的特性，比如请求处理，blueprint 路由创建和通过[Waterline](http://sailsjs.org/documentation/concepts/models-and-orm)的数据库集成。因为内核钩子绑定到Sails内核这样的话在每一个app中都是可用的。在你的代码中你可能很少需要去调用到内核钩子的方法。
2. **Project hooks**。这类钩子存在于Sails app的`api/hooks`文件夹下。工程钩子提供一种利用钩子系统特性的方法，对应的代码不需要在app之间共享。
3. **Installable hooks**。这类钩子安装在一个app的`node_module`文件夹下并使用`npm install`。可安装的钩子允许在Sails社区中的开发者去创建并安插--就像Sails app使用的模块一样。

## 更多阅读
* [Using hooks in your app](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/usinghooks.html)
* [The hook specification](http://sailsjs.org/documentation/concepts/extending-sails/hooks/hook-specification)
* [Creating a project hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html)
* [Creating an installable hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/installablehooks.html)



<docmeta name="displayName" value="Hooks">
<docmeta name="stabilityIndex" value="3">
