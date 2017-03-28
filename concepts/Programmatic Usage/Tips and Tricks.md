# 可编程方法的提示和技巧
当可编程地加载一个Sails app的时候，你会经常地想要关闭某些不常使用的钩子，不管是出于优化的考虑还是为了确保最小化Sails app和Node脚本之间的干扰。为了关闭一个钩子，你需要在一个`hooks`的目录中设置它为`false`，并将其作为第一个参数发送给`.load()`或`.lift()`。

另外，你也会经常想要关闭Sails [globals](http://sailsjs.org/documentation/concepts/globals)，*尤其当同时加载多个Sails app的时候*。因为在同一个进程中的所有Node app都会共享相同的globals，所以启动多个Sails app的时候关掉globals是准不会有错的，这可以避免模型、控制器和其他app-wide实体的冲突。

```javascript
// Turn off globala and commonly unused hooks in programmatic apps
mySailsApp.load({
  hooks: {
     grunt: false,
     sockets: false,
     pubsub: false
  },
  globals: false
})
```

最后，注意的是当你能够使用Sails构造器来创建和启动足够多的Sails apps的时候，每一个app只能启动一次。一旦你在某一个app中调用`.lower()`，那么你不能再启动它的。

<docmeta name="displayName" value="Tips and Tricks">
