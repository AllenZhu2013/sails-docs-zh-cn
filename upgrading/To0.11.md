# 升级到v.0.11

v0.11带有许多辅助的改进，也包括一些内核文件的内部清理。最大的改变是Sails 内核使用了Socket.io v1.

基本上该版本不会影响到现在的代码，但是仍然也有一些重要的不同和新特性。我们罗列在下面。

## 不同点
#### 升级Socket.io / Sails.io浏览器客户端
旧的v0.9 socket.io客户端不再工作，所以你需要从v0.9或v0.10升级你的sails.io.js客户端到v0.11。

为了升级，你只需要删除你的sails.io.js客户端并安装新的。我们绑定一个有用的生成器来帮你做这些操作，假设你的sails.io.js客户端是在默认的位置：`assets/js/dependencies/sails.io.js`(也就是你没有移动过它或者重命名它)：

 ```sh
sails generate sails.io.js --force
```

#### onConnect生命周期回调

> **tldr;**
>
 > 从`config/sockets.js`中删除你的`onConnect`函数。

`onConnect`生命周期回调已经被弃用了。相反，当一个新的套接字连接的时候如果你需要做些事情的话，那么你就需要从一个新连接的客户端中发送一个请求去做这些事情。`onConnect`的目的当时是为了优化性能(消除了需要做服务器来回的额外初始化)，但是现在它的使用会导致混淆和竞争的情况。如果你极度地需要消除服务器的来回程，那么你可以直接绑定一个操作在你的bootstrap函数(`config/bootstrap.js`)中的`sails.io.on('connect', function (newlyConnectedSocket){})`。然而不鼓励这样做。除非你正在面临着*真正的*产品性能问题，否则你应该使用上面提到的策略来给你"on connection"逻辑(也就是在socket连接之后从客户端发送一条初始化的请求)。Socket 请求是轻量级的，所以不会添加任何有形的开销到你的应用中，并且它还会让你的代码变得更加有预测性。

#### onDisconnect生命周期回调
为了支持`afterDisconnect`，`onDisconnect`生命周期的回调已经被弃用了。

如果你以前在使用`onDisconnect`，那么你可能不得不改变`session`然后手工调用`session.save()`。在v0.11中，这个工作也是基本一样，除了`afterDisconnect`接收一个额外的第三个参数：一个回调函数。这样的话，你只需要调用提供的回调函数当你的`afterDisconnect`逻辑结束的时候，所以Sails能够自动地持续你对session做的任何改变。最后，正如你料想的，你不再需要手工调用`session.save()`--它现在会自动调用(就像在一个正常的route、action、policy中的`req.session`)。

在config/sockets.js中重命名你的onDisconnect函数，如下：

> **tldr;**
> Rename your `onDisconnect` function in `config/sockets.js` with the following:
>
> ```
> afterDisconnect: function (session, socket, cb) {
>   // Be sure to call the callback
>   return cb();
> }
> ```


#### 在config/sockets.js中其他的配置
在Socket.io v1中的许多配置已经发生改变了，所以你需要按照如下更新你的config/sockets.js文件：

+ 如果在你的app中的`config/sockets.js`文件中没有自定义任何选项的话，你可以安全地删除或者注释掉整个文件并让Sails默认地执行它们的逻辑。否则参考新的[Sails sockets documentation](http://sailsjs.org/#!/documentation/reference/sails.config/sails.config.sockets.html)，确保你的配置仍然有效以避免不必要的信息丢失。
+ 如果你在一个*不支持严格会话*(包含Heroku)的环境中扩展到多服务器，那么你需要在你的`config/socket.js`和你的客户端中设置你的`transports`为`['websocket']`--更多信息参考[our Scaling doc](http://sailsjs.org/#!/documentation/concepts/Deployment/Scaling.html?q=configuring-your-app-for-a-clustered-deployment)。
+ 如果你以前在使用一个自定义的`authorization`函数来限制你套接字连接，那么你现在想要使用`beforeConnect`。虽然Socket.io v1已经启用了authorization，但是`beforeConnect`(从Engine.io中映射到`allowRequest`选项)的原理也是和它一样的。
+ 如果你正在使用其他底层直接传递给socket.io v1的套接字配置，请确保并检查[在sailsjs.org上的包含了所有新的配置选项的细节的参考页面](http://sailsjs.org/#!/documentation/reference/sails.config/sails.config.sockets.html)

#### "firehose"
为了测试套接字的“firehose”特性已经被弃用了。如果你不知道那是什么意思，也没啥事。虽然它的基本用法还是会持续一段时间，但是它很快就会从内核中删除，并且你不应该在你的app中依赖它。它应用到下面的这些方法：

+ sails.sockets.subscribeToFirehose()
+ sails.sockets.unsubscribeFromFirehose()
+ sails.sockets.drink()
+ sails.sockets.spit()
+ sails.sockets.squirt()

> 如果你还想要用"firehose"，请在[Twitter上让Mike知道](http://twitter.com/mikermcneil)(它可以作为一个隔离的钩子使用)。

#### 在子文件夹下的配置文件
在Sails `config`文件下的所有文件没有优先于彼此一直是我们的目的，包括那些只是用于文件组织的文件名和子文件夹(除了`local.js`和`env`、`locale`子文件夹)。但是，在以前的Sails版本中，保存配置文件到子文件夹下将会产生一个影响，那就是将文件名称作为关键词添加到`sails.config`中去，所以如果你保存一些配置到`config/foo/bar.js`,那么这些配置将会在sails.config.bar的命名空间下。这不是我们想要的并且会有潜在的混淆风险因为1) 目录名称被忽略掉，2) 移动文件将会导致配置关键词发生改变。这个问题在v0.11.x已经被修复掉:在子文件夹的配置文件将会被当做和在根目录`config`文件夹下一样。如果你因为各种原因需要依赖旧的行为，你也许可以在你的`.sailsrc`文件设置`dontFlattenConfig`为`true`,但是我们强烈建议你在`module.exports`中通过设置你想要的关键词来代替你自己设置的配置；比如`module.exports.foo = {...}`。更多细节请参考[issue #2544](https://github.com/balderdashy/sails/issues/2544)。

#### Waterline现在使用Bluebird
在v0.11, Waterline现在支持Bluebird(代替了q)的promises.如果你正在使用`.exec()`那么你不会收到影响--只有你在使用`.then()`的时候才会。更多信息请参考https://github.com/balderdashy/sails/issues/1186。


## 新特性
Sails v0.11同时也带来了一些我们希望你清楚的新特性:
#### 用户级别的hooks
钩子现在可以直接从NPM中安装了。

这意味着你现在可以在你的终端中用一条命令就可以安装钩子。比如，以[@sgress454](https://twitter.com/sgress454)制作的`autoreload`[钩子](https://github.com/sgress454/sails-hook-autoreload)为例，该钩子会监控你的后台代码的变化这样你就不需要每次修改你的控制器、路由或者模型等之后杀掉进程并重新启动服务器。

为了安装`autoreload`钩子，运行下面的命令：

 ```sh
npm install sails-hook-autoreload
```
这只是其中的一个例子。也许你已经知道，钩子在Sails中是最低级的可插拔的抽象事物。它们允许作者去接入启动进程，监听事件，注入自定义的“shadow”路由，总之会利用原始访问到实时运行的Sails。大部分你熟悉的Sails特性实际上已经都被实现成“内核”钩子一年多了，包含：
+ `blueprints` (提供blueprint API)
+ `sockets` (提供socket.io集成)
+ `grunt` (提供Grunt集成)
+ `orm` (提供Waterline ORM的集成,以及导入你的工程的适配器和模型等)
+ `http` (提供一个HTTP server)
+ 以及其他16个钩子。

你可以在http://sailsjs.org阅读更多关于如何在[新的可提高的“扩展Sails”](http://sailsjs.org/#!/documentation/concepts/extending-sails)中写出你自己的钩子的文档。

#### Socket.io v1.x

升级到Socket.io v1.0实际上不会影响你的应用层代码，因为你正在使用Sails自己提供的层抽象功能；任何从`sails.sockets.*`封装的方法以及"之上"的方法(resourceful pubsub, blueprints)。如果你正在你的app中使用根本的socket.io方法,或者只是好奇Socket.io v1.0改变了些什么,那么可以从Guillermo和ocket.io团队审阅[完整的Socket.io 1.0升级文档](http://socket.io/docs/migrating-from-0-9/)。

#### Ever-increasing modularity
作为Socket.io v1.0升级的一部分,我们将内核`sockets`钩子单独拉出一个独立的代码库。者允许我们写出一些有模块化的、钩子专用的socket.io拦截器的测试，这会让事情变得更容易维护、定制化和重写。这也允许钩子可以按照自己的步伐来成长并将相关的问题放在自己的空间下。

考虑到在接下来的几个月里从Sails内核的代码库中拉取其他的钩子来做测试，那么这会让Sails内核更加轻量级、更快以及更具有扩展性，拥有更少的内核依赖性以及更短的启动时间和更快速的`npm安装`。


#### 关于测试的“虚拟”请求拦截器和sails.request()方法
在从内核拉取出`sockets`钩子的过程中，拦截请求的逻辑已经被标准化了并且代码是位于Sails内核中。因此`sails.request()`方法将会变得更加有用.

这个方法允许你在Sails中直接与请求拦截器通信而不需要运行你的服务器在某个端口上。这与Sails从Socket.io映射入口消息到有类似`req`和`res`流的“虚拟请求”一样的机制。sails.request()使用的最大情形是在写快速运行单元的时候以及集成测试，但是它用在代理挂载的app(或者"sub-apps")也很便利。

比如，这里有一个例子(使用的是mocha)，讲解如何测试你的app的路由的一个：


 ```js
var assert = require('assert');
var Sails = require('sails').Sails;

before(function beforeRunningAnyTests (done){

  // Load the app (no need to "lift" to a port)
  sails.load({
    log: {
      level: 'warn'
    },
    hooks: {
      grunt: false
    }
  }, function whenAppIsReady(err){
    if (err) return done(err);

    // At this point, the `sails` global is exposed, although we
    // could have disabled it above with our config overrides to
    // `sails.load()`. In fact, you can actually use this technique
    // to set any configuration setting you like.
    return done();
  });
});

after(function afterTestsFinish (done) {
  sails.lower(done);
});

describe('GET /hotpockets', function (){

  it('should respond with a 200 status code', function (done){

    sails.request({
      method: 'get',
      url: '/hotpockets',
      params: {
        limit: 10,
        sort: 'price ASC'
      }
    }, function (err, clientRes, body) {
      if (err) return done(err);

      assert.equal(clientRes.statusCode, 200);
      return done();
    });

  });
});
```

#### config/env/ subfolders
在v0.10.x,我们添加了`config/env`文件夹(感谢[@clarkorz](https://github.com/clarkorz)), 在该文件夹下你可以添加在恰当的环境下才会加载的配置文件(比如`config/env/production.js`只会在production环境下加载, `config/env/development`只会在开发环境下加载)。 在v0.11.x我们增加了为每个环境指定整个子文件夹的功能。比如当环境被设置为产品模式的时候所有保存在`config/env/production`目录下的文件都会加载并会被合并到其它顶层配置中去。注意如果同时存在一个`config/env/production`文件夹和一个`config/env/production.js`文件，那么会优先采用`config/env/production.js`的设置.另外`local.js`总是会合并到其他配置文件和`.sailsrc`规则的头部。

#### 问题?
如果你在升级的过程中遇到问题，或者如果上面所讲的注意事项没什么意义，请联系我们，我们会尽我们所能帮助你的。

最后，给那些从v0.10在8月发布以后贡献过这个工程的你们：我们不能强调我们多么重视你的持续支持和鼓励。虽然这里还有大量的问题、pull request、文档tweaks和问题，但是所有的这些恰恰让我们知道我们一直是在一起的 :)

谢谢！

-[@mikermcneil](https://github.com/mikermcneil/), [@sgress454](https://github.com/sgress454/)和[@particlebanana](https://github.com/particlebanana/)

<docmeta name="displayName" value="To v0.11">
