# 创建一个工程钩子
工程钩子也就是自定义的Sails钩子，存在于一个应用中的`api/hooks`文件夹。当你想要利用钩子的特性，比如像[defaults](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/defaults.html)和[routes](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/routes.html)的代码在一个单一app中被多个组件使用的时候就很有用。如果你想要在多个Sails app中重用一个钩子，那么请查看[creating an installable hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/installablehooks.html)。

为了创建一个新的工程钩子：

1. 选择一个你的新钩子的名字。它不能与任何的[内核钩子名字](https://github.com/balderdashy/sails/blob/master/lib/app/configuration/default-hooks.js)相冲突。
2. 使用钩子的名称在你的app的`api/hooks`文件夹下创建一个新的文件夹。
3. 添加一个`index.js`文件到那个文件夹下。
4. 在`index.js`中根据[hook的技术规范](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec)写你的钩子代码。

你的新文件夹可能也包含其他文件，这些都可以通过`require`加载进你的钩子中；只有`index.js`才可以自动地被Sails读取。

如果不创建文件夹，你也可以在你的app的`api/hooks`文件夹下创建一个名为`api/hooks/myProjectHook.js`的文件。

#### 测试你的钩子是否加载顺利
为了测试你的钩子正在被Sails加载，使用`sails lift --verbose`启动你的app。如果你的钩子被加载了，那么你会在日志里看到这样的一条消息：

`verbose: your-hook-name hook loaded successfully.`

* [Hooks overview](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)
* [Using hooks in your app](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/usinghooks.html)
* [The hook specification](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec)
* [Creating an installable hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/installablehooks.html)


<docmeta name="displayName" value="Project Hooks">
<docmeta name="stabilityIndex" value="3">
