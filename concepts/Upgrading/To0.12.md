# 升级到v.0.12
Sails v0.12的升级伴随着Socket.io和Express的升级，以及一些bug的修复和性能的提升。你可以发现这个版本大部分向后兼容v0.11，但是还是有一些大的改动--`sails.sockets.*`方法的改变也许会或多或少地影响你的app。下面的升级指南大部分都是在解决这些改变，所以如果你从一个已有的v0.11 app中升级并且使用了`sails.sockets`方法，假使它影响到了你的app那么请确保已经仔细阅读了下面的内容。除此之外，在一个已经存在的工程中运行`sails lift`应该还是能工作的。

下面的章节提供了改动地方和修复bug、性能提升和新特性的一个高层概述，同时也包含了如何从v0.11升级到v0.12的一个基本指导。

## 安装更新
在你的Sails app的根目录下运行下面的命令：

 ```bash
npm install sails@0.12.0 --force --save
```

`--force`标志将会重写安装在你的`node_modules/`文件夹下已经存在的Sails依赖成Sails v0.12，`--save`标志将会更新你的package.json文件让以后你的npm安装软件的时候会使用新的版本。

## 升级后需要立即做的事情
+ 如果你的app使用了`socket.io-redis`适配器，那么请升级它到至少是1.0.0(`npm install --save socket.io-redis@^1.0.0`)
+ 如果你的app在前端使用了Sails socket client(比如`assets/js/dependencies/sails.io.js`)，那么也请升级到最新版本(`sails generate sails.io.js --force`)。


## 在v0.12中的改变预览
> 完整的改变列表请参考[Sails](https://github.com/balderdashy/sails/blob/master/CHANGELOG.md)的改变日志，其它(包括[Waterline](https://github.com/balderdashy/waterline/blob/master/CHANGELOG.md), [sails-hook-sockets](https://github.com/balderdashy/sails-hook-sockets/blob/master/CHANGELOG.md)和[sails.io.js](https://github.com/balderdashy/sails.io.js/blob/master/CHANGELOG.md))的也是一样。

+ 安全性增强：更新了一些潜在的弱点的依赖
+ 反向路由功能现在内嵌到Sails的内核通过新的方法--[sails.getRouteFor()](http://sailsjs.org/documentation/reference/application/sails-get-route-for)和[sails.getUrlFor()](http://sailsjs.org/documentation/reference/application/sails-get-url-for)
    + 普遍地提升了多节点支持底层的`sails.socket.*`方法，并且对相关的最新socket.io升级做出了些额外的调整和提高。添加一个更加小巧的Redis集成在`socket.io-redis`的顶部，使用redis客户端去实现跨服务器通信而不需要用多个socket 客户端。
    + 清空`sails.socket.*`方法的API，规范化重载函数和那些在多服务器部署会导致问题的被弃用的方法。
    + 添加几个全新的方法：`.leaveAll()`, `.addRoomMembersToRooms()`,和`.removeRoomMembersFromRooms()`。
    + `sails.sockets.id()`现在变成`sails.sockets.getId()`(向后兼容w/这个弃用的消息)
    + 新的Sails app带有更新版本的`sails.io.js`(JS Sails socket client)。这个升级绑定了最新版本的`socket.io-client`，也包含了一些更高级的功能(包含了为所有的虚拟套接字请求指定通用头部的能力)。
    + 升级到最新的`grunt-contrib-*`版本依赖(消除了许多NPM弃用的警告和提供更好的错误信息)。
    + 如果你在使用NPM v3,当运行`sails new`的时候会等于运行`npm install`而不是以前的符号链接到你的新的app初始化依赖中去。这个也许会比你之前使用的变慢了，但是这是必须的改变因为NPM处理嵌套的依赖的方法已经改变了。虽然内核维护者[一直在找寻](https://github.com/npm/npm/issues/10013#issuecomment-178238596)一个更好的长期解决办法，但是同时如果你运行很多次`sails new`并且速度慢困扰着你，那么考虑临时地降级到npm的早期版本(v2.x)。如果你安装的NPM版本小于3.0，那么Sails将会继续利用那经典地符号链接策略。


## 套接字方法
毫无疑问，在Sails v0.12的最大改变就是由`sockets`钩子暴露的底层`sails.sockets`方法的API。为了确保Sails app在[多服务器环境](http://sailsjs.org/documentation/concepts/realtime/multi-server-environments)(aka "multi-node" or "clustered")中完美无暇地执行，一些[底层方法](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)将会被弃用并且会添加一些新的方法。

下面的`sails.sockets`方法将被弃用：

+ [.emit()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-emit)
+ [.id()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-id)(重命名为[.getId())](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/get-id))
+ [.socketRooms()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-socket-rooms)
+ [.rooms()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-rooms)
+ [.subscribers()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-subscribers)

如果你在你的app中使用上面的任一个方法，虽然它们在v0.12中仍然还可以工作但是你最好尽快地将它们替换掉因为它们在下一个版本中将会被删除掉的。更多信息参考每个方法的独立文档页。

## 资源丰富的发布订阅
资源丰富的[.subscribers()](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub/subscribers)方法已经被弃用因为和[sails.sockets.subscribers()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-subscribers)相同的原因。如果你在你的代码中使用它那么参考它的文档中说明替换的方法指导。

## Waterline(ORM)更新
Sails v0.12带有最新版本的Waterline(ORM) v0.11.0.这里有两个改变的API值得注意：

##### .save()不再提供其回调函数的第二个参数
`.save()`实例方法的回调不再有第二个参数。提供这第二个参数的需求让`.save()`变得性能差，尤其app工作在成百万的记录中。这个改变解决了那些问题通过消除需要构建的冗余请求以及避免你的数据库不得不处理他们。

如果在你的代码中某些地方有这种类似的代码：

```javascript
sierra.save(function (err, modifiedSierra){
  if (err) { /* ... */  return; }

  // ...
});
```

那么现在需要替换成这样：

```javascript
sierra.save(function (err){
  if (err) { /* ... */  return; }

  // ...
});
```

##### 内建的时间戳自定义的专栏或字段
你现在可以为内建的`createdAt`和`updatedAt`属性配置一个自定义的专栏名称(也就是Mongo/Redis中的字段名字)
。在过去，上层的`autoCreatedAt`和`autoUpdatedAt`模型设置可以指定为`false`来禁用自动注入`createdAt `和`updatedAt`。现在这个功能还是可以能用，但是现在你还可以为它们指定字符串值。如果一个字符串被指定了，它将被理解为自定义的专栏(字段)名称来用在自动化的时间戳上。

```javascript
{
  attributes: {},
  autoCreatedAt: 'my_cool_created_when_timestamp',
  autoUpdatedAt: 'my_cool_updated_at_timestamp'
}
```

如果你使用[workaround suggested by @sgress454 here](http://stackoverflow.com/a/24562385/486547)，你也许可以利用这个简单的方法。

## SQL适配器性能
[Sails-PostgreSQL](https://github.com/balderdashy/sails-postgresql)和[Sails-MySQL](https://github.com/balderdashy/sails-mysql)进行补丁更新，该补丁可以有效地提高性能当在populating关联的时候。感谢[@jianpingw](https://github.com/jianpingw)深挖代码并发现一个处理数据库会记录很多次的bug。如果你使用这些适配器中的一个，那么升级到`sails-postgresql@0.11.1`或`sails-mysql@0.11.3`后会给你一个性能的巨大提升。


## 贡献
虽然不是技术上发布的一部分，但是Sails v0.12是由对贡献者可用的的工具和资源的一些主要提升完成的。大部分的内核钩子现在都完全文档化了([controllers](https://github.com/balderdashy/sails/tree/master/lib/hooks/controllers)|[grunt](https://github.com/balderdashy/sails/tree/master/lib/hooks/grunt)|[logger](https://github.com/balderdashy/sails/tree/master/lib/hooks/logger)|[cors](https://github.com/balderdashy/sails/tree/master/lib/hooks/cors)|[responses](https://github.com/balderdashy/sails/tree/master/lib/hooks/responses)|[orm](https://github.com/balderdashy/sails/tree/master/lib/hooks/orm))，并且团队还把对Sails工程有贡献的人放在一起成一个[Code of Conduct](https://github.com/balderdashy/sails/blob/master/CODE-OF-CONDUCT.md)。

对于贡献者最大的改变是[更新的贡献指导](https://github.com/balderdashy/sails/blob/master/CONTRIBUTING.md)，包含了新的、流线型的feature/enhancement提议和合并feature、enhancements、补丁到内核的流程。因为Sails框架在不断成长(包括代码量和用户量)，建立一个清晰的流程来review和合并问题贡献者、代码贡献者、以及文档贡献者是很有必要的。

## 文档
该版本带来一个对官方参考文档的深度清理工作，以及对在线文档http://sailsjs.org/documentation的一些小的可用性改进。现在网站的[日本版本](http://sailsjs.jp/)已经可用了，另外其他[四个版本(Korean，Brazillian Portugese，Taiwanese Mandarin，和Spanish)的翻译工程](https://github.com/balderdashy/sails-docs#in-other-languages)已经在进行中了。

另外Sails.js工程有一个[官方博客](http://blog.sailsjs.org/)。Sails.js博客是所有更新和关于Sails.js公告的来源，也包括所有相关的工程比如Waterline、Skipper和机械规格表。

## 需要帮忙？
如果你升级到v0.12之后遇到了一个未曾见过的问题，请审查我们的投稿指南并[提交问题到Sails GitHub代码库中](https://github.com/balderdashy/sails/blob/master/CONTRIBUTING.md)。


<docmeta name="displayName" value="To v0.12">
