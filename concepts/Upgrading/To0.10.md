# 升级到v.0.10
对于大部分，在一个已经存在额工程v0.9上运行sail lift应该是能够正常工作的。代码内核的贡献者采取了几个步骤尽量让升级变得简单，如果你跟着控制台提示的信息操作，一般都是没有问题的。

Sails v0.10有一些重大的改动。下面的小节就是简要地概述改动的地方以及修复的bug和一些增强的特性或新的特性，然后是一份如何升级你的v0.9到v.10的指南。

## 文件上传
Connect多部件的中间件很快会被[官方废弃掉](http://www.senchalabs.org/connect/multipart.html)。但是因为这个模块在Sails v0.9和Express v3中被用作一个内建的HTTP body分析器，所以对于依赖于`req.files`的Sails v0.9的工程来说这是一个重大的变化。

默认在v0.10中Sails包含了[skipper](https://github.com/balderdashy/skipper)，它是一个允许流文件上传而不需要缓存临时文件到磁盘的body 分析器。对于普通的文件上传的用例，Skipper 默认支持上传到本地磁盘(通过skipper-disk)，但是流上传可以插入到任何它支持的适配器中。

更多例子和文档请参考Skipper库。

### 为什么？
body分析器是为了分析进入的多部件的HTTP请求的“body”。有时，“body”包含文本参数，但是有时，它包含待上传的文件。

Connect多部件是一份伟大的代码，它同时支持文件上传和文本参数在多部分请求中。但是像它的类型大部分模块，它通过缓存文件上传到磁盘来实现。这可以快速地覆盖一个服务器可用的磁盘空间，在大部分情况下暴露一个严重的DoS攻击的弱点。

Skipper不但是唯一支持**流文件**上传的，而且维护支持在请求主体的元数据(也就是JSON/XML/urlencoded请求body参数)。它使用少量的试探法来确保只有你期望的文件可以插入和接收通过blob适配器，其他文件都被忽略掉。

> **重要！！**

> 为了让Skipper工作，你必须在文件参数之前包含所有的文本参数。一旦Skipper看到第一个文件字段，它将会停止等待文本参数(这是为了避免缓存那些没用的文件数据)

### 配置一个不同的body分析器
就像在Sails的其他配置一个，你可以使用任何Connect、Express、Sails兼容的body分析器。为了能切换到**connect-multipart**或者其他任何的body分析器(比如**formidable**或**busboy**)，只需要改变你的app的http配置。

## Blueprints
新增了一个Blueprints动作`findOne`。比如你有一个`FooController`和`Foo`模型，然后发送一个请求到`/foo/5`，那么在你的`FooController`将会执行`findOne`动作。如果你没有`findOne`动作，那么`find` blueprint动作将会被用来替代它。发送到`/foo`的请求仍然会运行`find`控制器/blueprint动作。


## Policies
策略的工作与v0.9的基本完全一样，不过现在的版本新考虑了一种情况：为了能更具体地介绍上面提到的blueprint动作`findOne()`，你想要确保在你的策略映射配置中明确地操作它。

比如，让我们假设你的v0.9 app有一个文件`policies.js`，其配置阻止在你的`DoveController`中访问`find`动作：

```
module.exports.policies = {
  '*': true,
  DoveController: {
    find: false
  }
};
```

假设剩下的blueprint路由都是使能的，这样将会阻止像`/dove`和`/dove/14`这样的请求访问。但是在现在的v0.10，因为`/dove/14`将会实际运行在`findOne`动作，我们应该明确地这样操作：

```
module.exports.policies = {
  '*': true,
  DoveController: {
    find: false,
    findOne: false
  }
};
```

## 发布订阅
### 概述
+ 在客户端上的`message`套接字(也就是“comment”)事件现在是`modelIdentity`("modelIdentity"依赖于从不同地方调用`publish*()`的模型。)
+ 客户端不再通过blueprint路由订阅model-certain事件。为了监听这类创建的事件，使用`Model.watch()`。
+ 之前叫做`create`, `update`, 和`destroy`的事件现在是`created`, `updated`,和`destroyed`。

### 细节
发布订阅的最大的改变是Socket.io事件在发射它们的模型名称下面发射了。以前，你的客户端监听`message`事件然后不得不通过它包含的数据决定它来自哪个model：

```
socket.on('message', function(cometEvent) {
   if (cometEvent.model == 'user') {
     // Handle inbound messages related to a user record
   }
   else if (cometEvent.model === 'product') {
     // Handle inbound messages related to a product record
   }
   // ...
}
```

现在，你订阅到模型的ID：

```
socket.on('user', function(cometEvent) {
  // Handle inbound messages related to a user record
});

socket.on('product', function (cometEvent) {
  // Handle inbound messages related to a product record
});
```

这个有助于结构化你的前端代码。

订阅客户端到你的模型的方法也改变了。以前你指定你是否订阅到模型类或者一个多个模型实例基于你传递给Model.subscribe的参数。这在一个方法来做两件不同的事情特别有效率。

现在，你使用`Model.subscribe()`来仅仅订阅模型实例。你也可以指定时间的“contexts”，或者你想要得知的类型。比如，如果你只想要获取消息更新一个实例，你调用`User.subscribe(req, myUser, 'update')`。如果在调用.subscribe()中没有给任何的`context`，然后指定给该模型类的自动订阅属性的所有`contexts`将会被使用。

为了订阅到模型创建事件，你现在可以使用`Model.watch()`。在订阅上，当使用blueprint路由创建一条新记录的时候，你的客户端每次都会收到消息，并且会自动订阅到新的实例。

记住，当工作在blueprint的时候，客户端不再自动订阅到`class room`。这个必须手动做。

最后，如果你想要看到从所有模型的所有的订阅发布消息，你可以访问`firehose`，这是一个开发工具，可以广播与你的模型相关的所有消息。你可以订阅到firehose使用`sails.sockets.subscribeToFirehose(socket)`。或者在前端通过使用一个套接字请求到`/firehose`。`firehose`将会广播一个`firehose`事件无论是模型创建、更新或者删除等。这个有效地替换掉了在以前Sails版本使用的`message`事件。

关于新的发布订阅动作的例子请参考[SailsChat](https://github.com/balderdashy/sailschat)。


## 现在生命周期的回调参数可以类型转换了
以前，当使用`schema: true`，如果你发送一个属性值到一个`.create()`或`.update()`，但是值并不匹配在模型属性中定义的类型，你传递的值仍然可以访问到你的模型的生命周期回调。

在Sails/Waterline v0.10，这种情况将不会再发生。传递给`.create()`和`.update()`在你的生命周期回调之前会被类型转换。受影响的声明周期回调包括`beforeUpdate()`, `beforeCreate()`,和`beforeValidate()`.

## beforeValidation()现在变成是beforeValidate()
如果你正在使用`beforeValidation`或者`afterValidation`模型生命周期回调在你的任何模型中，你应该将它们改变成`beforeValidate`或`afterValidate`。在Waterline中的这个改变是为了匹配其他生命周期回调的样式(比如`beforeCreate`,`afterUpdate`等)。

## .done() vs. .exec()
`.done()`的旧含义已经被废弃了。

在版本小于等于0.8的Sails中，执行一个ORM请求的语法是`Model. [ … ] .done( cb )`。在v0.9中，当支持promise时，`Model. [ … ] .exec( cb )`替换掉以前的写法。因为`.done()`在promise中有着特殊意义。然而，`.done()`的以前用法仍然未改变的被留下以让从0.8到0.9的升级变得容易。

但是在0.10版本中，`.done()`的原始含义被官方地废弃掉以允许有更健壮promise实现，并可插拔的promise库支持(比如选择 `Q`和`Bluebird`等)

## 关联
Sails v0.10介绍了数据模型之间的关联。因为我们在关联中做的工作是大量附加的，所以你的已经存在的模型仍然能够工作。也就是说，这是一个很有用的新特性允许你写更少的代码并让你的app更加可维护，所以我们建议使用它。为了学习如何在Sails中使用关联，请查看文档。

关联实际上仅仅是特殊的属性。取代字符串或者整型值，你可以指定一个模型的实例或者一个模型实例的集合。你可以考虑这类型比如object (`{...}`)或者数组(`[{...}, {...}]`) 你可能存储为JSON在一个NoSQL的数据库。唯一不同的是，在Sails中，这个可以工作在任何支持的数据库上，甚至允许你去populate


## 生成器
Sails已经支持生成代码有那么一会了(比如`sails generate controller foo`)，但是在v0.10中，我们想要让这个特性变得可扩展开放和在Sails社区中人人可用。 考虑到这一点，v0.10带有一个完全重写的命令行工具和可插拔的生成器。你是否想要能够运行`sails generate blog foo`来创造一个新的博客？创建一个`blog`生成器，添加你的模板，并配置生成器来拷贝新的模板。然后你可以释放它到社区通过发布到一个npm模块叫做`sails-generate-blog`。兼容Yeoman生成器也是以后我们想要实现的。

## 命令行工具
最大的改变是你怎样创建一个api。在过去你调用`sails generate new_api`，这将会生成一个新的生成器和模型叫做`new_api`在合适的地方。现在这个是使用`sails generate api new_api`来实现。

你仍然可以分别生成模型和控制器使用相同的CLI命令。

同时，`--linker`开关不再使用。在过去的版本中，如果`--linker`开关存在的话，它将会创建一个`myApp/assets/linker`文件夹，包含了`js`, `styles`和`templates`文件夹。在这个新的版本中，`myApp/assets/linker`不再创建。现在编译CoffeeScript和Less是默认的行为，默认编译`myApp/assets/js`和 `myApp/assets/scripts`文件夹。


## 自定义服务器端响应
在v0.10，你现在可以生成你自己自定义的服务器响应。

就像以前，我们自动为你创建的那些文件。与其生成`myApp/config/500.js`和其他`.js`响应文件在config目录下，它们现在可以生成在`myApp/api/responses/`。

为了迁移，你需要创建一个新的v0.10工程然后拷贝`myApp/api/responses`目录到你存在的app中。然后你修改合适的.js文件来反映任何客制化，你在你的响应逻辑中做的改变。


## 存储在临时的sails-disk数据库的传统文件
`Sails-disk`在新的Sails工程默认使用，现在存储的数据有点不同。如果你有一些临时数据存储在一个v0.9x工程中，你想要清除并开始刷新，可以这样做：

在你的工程根目录下面执行:

```
$ rm .tmp/disk.db
```

## 适配器/数据库配置
`config.adapters`(在`myApp/config/adapters.js`)现在是`config.connections`(在新的项目中，在`myApp/config/connections.js`中)。同时`config.model`现在是`config.models`。

你的app的默认`connection`现在应该被配置为一个字符`config.models.connection`。新的工程会生成一个`/config/models.js`，该文件包含默认的connection。

为了配置一个模型去使用专用的适配器，你现在必须在`connection`关键词中指定它们而不是在`adapters`。

比如：

```
module.exports = {

    connection: ['someMongoDatabase'],

    attributes: {
        name:{
            type     : 'string',
            required : true
        }
    }
};
```

## Blueprint/Controller配置
描述控制器配置的对象字面量应该重写，将下面的配置：

```
...
_config: {
  blueprints: {
    rest: true,
    ...
  }
}
```

修改为：
```
...
_config: {
    rest: true,
    ...
}
```

## 布局路径
在Sails v0.9中，你可以使用下面的语法来指定`auth/someLayout.ejs`作为你的自定义的布局当渲染一个视图时：

```
return res.view('auth/login',{
  layout: 'someLayout'
});
```
然而现在在Sails v0.10中，所有的布局文件路径都是相对的了。换句话说，布局文件的相对路径不再从视图自己的路径中解析--它现在总是从视图路径中解析。这样就更容易地理解哪个文件正在使用，尤其当布局文件有类似的名字的时候：

```
return res.view('auth/login', {
  layout: 'auth/someLayout'
});
```


<docmeta name="displayName" value="To v.0.10">
