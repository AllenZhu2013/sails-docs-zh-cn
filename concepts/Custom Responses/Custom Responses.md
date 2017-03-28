# Custom Responses
### 概述
Sails V0.10允许定制化服务器的响应。Sails默认自带少数最常见的响应类型。它们都放置于`/api/responses`目录。为了自定义这些响应，简单地修改appropriate.js文件。

下面是一个简单的例子，

```javascript
foo: function(req, res) {
   if (!req.param('id')) {
     res.status(400);
     res.view('400', {message: 'Sorry, you need to tell us the ID of the FOO you want!'});
   }
   ...
}
```

这段代码处理一个不合理的请求通过发送一个400错误以及一段简短的信息来描述问题。但是，这段代码有一些弊端，主要有：

+ 它是*非标准化的*；这段代码专用于这个实例，然后我们会努力地保持在任何地方都有相同的格式；
+ 它是*非抽象的*；如果我们想要在别地地方使用这段代码，我们不得不拷贝粘贴。
+ 响应是*非内容可协商的*；如果客户端期望收到一个JSON响应，那么这就有点运气不好了。

那么我们将其改成如下：

```javascript
foo: function(req, res) {
   if (!req.param('id')) {
     res.badRequest('Sorry, you need to tell us the ID of the FOO you want!');
   }
   ...
}
```

这种方法有许多优点：
+ 错误的payload是标准化的；
+ 产品和开发模式下的日志考虑在内了；
+ 错误代码是一致的；
+ 考虑到了内容可协商(JSON和HTML)；
+ 可以快速编辑那个合适的响应文件以完成API微调；

### 响应方法和文件
任何`.js`文件都被保存在`/api/responses`目录下，然后都可以在你的控制器中通过调用res.[responseName]来使用。比如，当调用`res.serverError(errors)`的时候将执行`/api/responses/serverError.js`。在响应脚本内部，请求和响应对象作为`this.req`和`this.res`都是可用的；这样就允许实际的响应函数可以携带任意的参数(比如`serverError`的`错误`参数)。


<docmeta name="displayName" value="Custom Responses">
