# 创建一个可安装的钩子
可安装的钩子也就是自定义的Sails钩子，存在于一个应用中的`node_modules`文件夹。当你想要在Sails app之间共享功能或者发布你的钩子到[NPM](http://npmjs.org/)上与Sails社区共享的时候特别有用。如果你只是想要创建一个钩子在一个Sails app中使用的话，请参考[creating a project hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html)。

为了创建一个可安装的钩子：

1. 选择一个你的新钩子的名字。它不能与任何的[内核钩子名字](https://github.com/balderdashy/sails/blob/master/lib/app/configuration/default-hooks.js)相冲突。
2. 在你的系统上新建一个名字为`sails-hook-<your hook name>`的新文件夹。`sails-hook-`前缀是可选的但还是建议你保持一致性。当钩子被加载的时候前缀会自动被Sails截掉。
3. 在文件夹中创建一个`package.json`文件。如果你在你的系统中已经安装了`npm`，那么你可以通过运行`npm init`来简单地生成这个文件并允许一些选项配置。否则，你得手工创建然后确保它至少包含了下面的信息：
```
{
    "name": "sails-hook-your-hook-name",
    "version": "0.0.0",
    "description": "a brief description of your hook",
    "main": "index.js",
    "sails": {
      "isHook": true
    }
}
```
如果你使用`npm init`去创建你的`package.json`，请打开该文件之后手动地加入`sails`关键词并包含`isHook:true`的内容。
4. 在`index.js`中根据[hook的技术规范](http://sailsjs.org/documentation/concepts/extending-sails/hooks/hook-specification)写你的钩子代码。

Your new folder may contain other files as well, which can be loaded in your hook via require; only index.js will be read automatically by Sails. Use the dependencies key of your package.json to refer to any dependencies that need to be installed in order for your hook to work (you may also use npm install <dependency> --save to easily save dependency information to package.json).

### 为你的钩子指定内部Sails使用的名称(进阶)
在某些例子中，尤其当使用一个[scoped NPM package](https://docs.npmjs.com/misc/scope)去重写一个内核钩子的时候，你会想要改变Sails内部使用的名称当它加载你的钩子的时候。你可以在你的`package.json`文件中使用`sails.hookName`配置选项。这个值应该是你想要加载进`sails.hooks`目录的钩子名称，所以你一般不会想要一个`sails-hooks-`的前缀。比如，如果你有一个`@mycoolhooks/sails-hook-sockets`的模块，你想要使用它去重写内核`sails-hook-sockets`模块，那么`package.json`应该是这样：

```
{
    "name": "@mycoolhooks/sails-hook-sockets",
    "version": "0.0.0",
    "description": "my own sockets hook",
    "main": "index.js",
    "sails": {
      "isHook": true,
      "hookName": "sockets"
    }
}
```
### 测试你的新钩子
在你分发你的可安装钩子给别人之前，你想要写一些测试来测试钩子。这个测试可以帮助你确保你的钩子与未来的Sails版本兼容并且有效地减少拉扯和破坏附近的对象。写出一份完整的测试指导超出了本文档的范围，但是下面介绍的步骤还是可以帮助你简单地写出一个测试的：

1. 在你的钩子的`package.json`问文件中添加Sails为一个`devDependency`：
```
"devDependencies": {
      "sails": "~0.11.0"
}
```

2. 使用`npm install sails`或者`npm link sails`(如果你已经全局地安装了Sails在你的系统中的话)安装你的钩子的依赖软件Sails。
3. 使用`npm install -g mocha`安装[Mocha](http://mochajs.org/)到你的系统中如果没有的话
4. 在你的钩子的主文件夹下添加一个`test`文件夹
5. 添加一个basic.file文件并带有下面的基本测试：
```
  var Sails = require('sails').Sails;

  describe('Basic tests ::', function() {

        // Var to hold a running sails app instance
    var sails;

        // Before running any tests, attempt to lift Sails
    before(function (done) {

      // Hook will timeout in 10 seconds
      this.timeout(11000);

      // Attempt to lift sails
        Sails().lift({
          hooks: {
            // Load the hook
            "your-hook-name": require('../'),
            // Skip grunt (unless your hook uses it)
            "grunt": false
          },
          log: {level: "error"}
        },function (err, _sails) {
          if (err) return done(err);
          sails = _sails;
          return done();
        });
    });

        // After tests are complete, lower Sails
    after(function (done) {

      // Lower Sails (if it successfully lifted)
      if (sails) {
        return sails.lower(done);
      }
      // Otherwise just return
      return done();
    });

    // Test that Sails can lift with the hook in place
    it ('sails does not crash', function() {
      return true;
    });

  });
```

6. 使用`mocha -R spec`运行测试并查看完整的结果
7. 完整资料请参考[Mocha](http://mochajs.org/)文档。

### 发布你的新钩子
假设你的钩子测试通过，并且假设你的钩子名字没有被其他的[NPM](http://npmjs.org/)模块使用，那么你就可以通过运行`npm publish`发布到社区与全世界共享你的模块。

* [Hooks overview](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)
* [Using hooks in your app](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/usinghooks.html)
* [The hook specification](http://sailsjs.org/documentation/concepts/extending-sails/hooks/hook-specification)
* [Creating a project hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/projecthooks.html)

<docmeta name="displayName" value="Installable Hooks">
<docmeta name="stabilityIndex" value="3">
