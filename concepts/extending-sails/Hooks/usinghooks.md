# 在一个Sails App中使用钩子
## 使用一个工程钩子
为了在你的app中使用工程钩子，首先创建`api/hooks`文件夹如果它不存在的话。然后[创建工程钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html)或者你想要在`api/hooks`文件夹下使用的钩子文件夹。

## 使用一个可安装的钩子
为了在你的app中使用可安装的钩子，只需要运行`npm install`加上你想要安装的钩子的包名(比如`npm install sails-hook-autoreload`)。你也可以直接地手工拷贝或者链接一个你[已经创建的可安装钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/installablehooks.html)的文件夹到你的`node_modules`文件夹。

## 调用钩子方法
一个钩子暴露的任何方法都可以在对象`sails.hooks[<hook-name>]`中可用。比如，`sails-hook-email`钩子提供一个`sails.hooks.email.send()`方法(注意前缀`sails-hook-`是被剥掉的)。参考一个钩子的文档看看它提供了哪些方法。

## 配置一个钩子
一旦你已经添加一个可安装的钩子到你的app中，你可以使用常规的Sails配置文件比如`config/local.js`，` config/env/development.js`或者你自己创建的自定义配置文件来配置钩子。钩子的设置都是直接使用钩子的名称，任何的前缀`sails-hook-`都会被剥离掉。比如，`sails-hook-email`的`from`设置名是`sails.config.email.from`。可安装的钩子的文档应该描述清楚哪些可用的配置选项。

## 改变Sails加载一个可安装钩子的方法
偶尔你可能需要改变Sails使用的可安装钩子的名称，或者改变钩子使用的配置关键词。有这么一种情况那就是如果你已经有了一个和可安装钩子同名的工程钩子或者如果你已经在使用一个配置关键词做别的事情。为了避免这些冲突，Sails提供`sails.config.installedHooks.<hook-identity>`配置选项。这个钩子的身份*总是*和钩子安装的文件夹的名称。

```
// config/installedHooks.js
module.exports.installedHooks = {
   "sails-hook-email": {
      // load the hook into sails.hooks.emailHook instead of sails.hooks.email
      "name": "emailHook",
      // configure the hook using sails.config.emailSettings instead of sails.config.email
      "configKey": "emailSettings"
   }
};
```

> 注意：你也许已经创建了`config/installedHooks.js`文件。

* [Hooks overview](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)
* [The hook specification](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec)
* [Creating a project hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html)
* [Creating an installable hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/installablehooks.html)



<docmeta name="displayName" value="Using Hooks">
<docmeta name="stabilityIndex" value="3">
