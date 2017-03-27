# `initialize(cb)`
`initialize`特性允许一个钩子去执行那些也许依赖于钩子或异步的启动任务。所有的Sails配置都保证在一个钩子的`initialize`函数运行之前完成。你也许会放在`initialize`中的任务例子是：

+ 登录到一个远程的API
+ 从一个会被钩子方法使用的数据库读取数据
+ 从一个用户配置的目录中加载支持文件
+ 等待其他的钩子首先去加载

就像所有钩子的特性，`initialize`是可选地并且可以不计入你的钩子定义中。如果实现了，`initialize`会携带一个参数：一个回调函数，在Sails结束加载的时候会去调用它：

```javascript
initialize: function(cb) {

   // Do some stuff here to initialize hook
   // And then call `cb` to continue
   return cb();

}
```

<<<<<<< HEAD
##### 钩子超时设置
默认，钩子有10秒的时间去完成`initialize`函数并在Sails抛出一个错误之前调用`cb`。这个超时时间可以通过设置`_hookTimeout`关键词来设置，超时时间的单位是毫秒。这个是在钩子的[defaults](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/defaults.html)中设置：
=======
##### Hook timeout settings

By default, hooks have ten seconds to complete their `initialize` function and call `cb` before Sails throws an error.  That timeout can be configured by setting the `_hookTimeout` key to the number of milliseconds that Sails should wait.  This can be done in the hook&rsquo;s [`defaults`](http://sailsjs.com/documentation/concepts/extending-sails/Hooks/hookspec/defaults.html):
>>>>>>> upstream/master

```
defaults: {
   __configKey__: {
      _hookTimeout: 20000 // wait 20 seconds before timing out
   }
}
```

##### 钩子事件和依赖
当一个钩子成功地初始化知乎，它会发射一个下面名字的事件：

`hook:<hook name>:loaded`

比如：
+ `orm`内核钩子发射`hook:orm:loaded`在它的初始化结束之后
+ 安装在`node_modules/sails-hook-foo`的钩子默认会发射`hook:foo:loaded`事件。
+ 相同的`sails-hook-foo`钩子，但是设置`sails.config.installedHooks['sails-hook-foo'].name`为`bar`的话会发射`hook:bar:loaded`事件
+ 安装在`node_modules/mygreathook`的钩子会发射`hook:mygreathook:loaded`事件
+ 安装在`api/hooks/mygreathook`的钩子会发射`hook:mygreathook:loaded`事件

你可以使用"hook loaded“事件来让一个钩子依赖于另一个钩子。为了这样做，你可以简单地将`sails.on()`的调用封装到你的钩子的`initialize`逻辑。比如，为了让你的钩子等待钩子`orm`钩子加载，你可以让你的`initialize`函数写成这样：

```javascript
initialize: function(cb) {

   sails.on('hook:orm:loaded', function() {

      // Finish initializing custom hook
      // Then call cb()
      return cb();

   });
}
```
为了让你的钩子依赖于多个钩子，你可以将所有需要等待的时间组成一个数组并调用`sails.after`：

```javascript
initialize: function(cb) {

   var eventsToWaitFor = ['hook:orm:loaded', 'hook:mygreathook:loaded'];
   sails.after(eventsToWaitFor, function() {

      // Finish initializing custom hook
      // Then call cb()
      return cb();

   });
}
```


<docmeta name="displayName" value=".initialize()">
<docmeta name="stabilityIndex" value="3">
