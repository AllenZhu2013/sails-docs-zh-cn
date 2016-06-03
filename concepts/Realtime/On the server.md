# 从一个服务器中发送实时消息到多个客户端
### 概述
Sails暴露出两个API用来与连接上的socket客户端通信：高层的[resourceful pubsub API](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，和底层的[sails.sockets API](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)。

### Resourceful PubSub
Resourceful PubSub (Published/Subscribe) API提供一个高层的方法来订阅sockets到Sails的模型类和实例。使用这个API完全有可能创建出一个非常棒的实时体验(比如一个聊天app)。Sails blueprints使用Resourceful PubSub来自动地将关于新的模型实例和已经存在实例的改变通知发送出去，但是你也可以在你自定义的控制器中使用它们。

##### 例子
创建一个User模型实例并通知所有感兴趣的客户端

```javascript
// Create the new user
User.create({
  name: 'johnny five'
}).exec(function(err, newUser) {
  if (err) {
    // Handle errors here!
    return;
  }
  // Tell any socket watching the User model class
  // that a new User has been created!
  User.publishCreate(newUser);
});
```


### `sails.sockets`
`sails.sockets`API允许直接与sockets进行底层通信，使用诸如[sails.sockets.join()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-join)(订阅一个socket到所有的消息并发送到一个特殊的”房间“)，[sails.sockets.leave()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-leave)(从一个房间中取消订阅)，以及[sails.sockets.broadcast()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-broadcast)(在一个或者多个房间中广播一条消息到所有的订阅者)这样的方法。

##### 例子
添加一个socket到房间”funSockets“

```javascript
sails.sockets.join(someSocket, "funSockets");
```

广播一条"hello"消息到"funSockets"房间。这条消息将会被所有满足下面条件的客户端sockets接收到：(1)在服务器上已经使用`sails.sockets.join()`添加到”funScokets“房间中的，(2)在客户端中使用[socket.on('hello', ...)]添加一条侦听”hello“事件的侦听器。

```javascript
sails.sockets.broadcast("funSockets", "hello", "Hello to all my fun sockets!");
```


### 参考
+ 参考[sails.io.js library](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)，学习如何在客户端使用sockets与你的Sails app通信。
+ 参考[sails.sockets](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)，学习如何从服务器发送消息到连接上的sockets。
+ 参考[resourceful pub-sub](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，学习如何使用Sails blueprints来自动地发送关于你的[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)改变的实时消息。
+ 访问[Socket.io](http://socket.io/)网站来学习更多关于Sails使用的底层库来实时通信的知识。

<docmeta name="displayName" value="On the server">
