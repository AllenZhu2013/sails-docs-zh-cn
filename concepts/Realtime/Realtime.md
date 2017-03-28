# 实时通信(aka Sockets)

### 概述
Sails app支持在客户端和服务器间进行全双工的实时通信。这意味着一个客户端(比如一个web浏览器网页或者标签)可以与一个Sails app维护一个永久的连接，并且消息可以在任何时候从客户端发送到服务器或者服务器发送到客户端。实时通信的两个使用场景是即时通信的实现以及多用户游戏。Sails在服务器使用[socket.io](http://socket.io/)库实现实时，在客户端使用[sails.io.js](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)库。在全部的Sails文档中，术语**socket**和**websocket**一般都是用来指在一个Sails app和一个客户端之间的双向、永久的通信通道。

通过sockets与Sails app通信类似于使用AJAX，这两种办法都允许一个网页不用通过刷新就能够与服务器交互。但是，sockets与AJAX有两个重要的不同点：第一，一个socket可以在页面打开之后一直保持连接到服务器，允许它维护*状态*(AJAX请求，像所有的HTTP请求，是*无状态的*)；第二，因为sockets总是自然连接的，所以Sails app能够在任何时候发送数据到一个socket中(因此有"实时"这个绰号)，然而AJAX只允许服务器只有在收到请求的时候才可以做出回应。

### 使用资源丰富的订阅/发布来实时更新模型
产生请求到Sails [blueprints](http://sailsjs.org/documentation/reference/blueprint-api)的sockets会自动地通过[resourceful pub-sub API](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)订阅它们想获取的关于模型的实时消息。你也可以在你自定义的控制器动作中使用这个API来发送消息到对某些模型感兴趣的客户端中。


##### 例子
连接一个客户端的socket到服务器，然后订阅`user`事件，接着请求`/user`订阅到当前和将来的User模型实例。

    ```html
<!-- Simply include the sails.io.js script, and a client socket will be created for you -->
<script type"text/javascript" src="/js/dependencies/sails.io.js"></script>
<script type"text/javascript">
// The automatically-created socket is exposed as io.socket.
// Use .on() to subscribe to the 'user' event on the client.
// This event is sent by the Sails "create", "update",
// "delete", "add" and "remove" blueprints to any socket that
// is subscribed to one or more User model instances.
io.socket.on('user', function gotHelloMessage (data) {
  console.log('User alert!', data);
});
// Using .get('/user') will retrieve a list of current User models,
// subscribe this socket to those models, AND subscribe this socket
// to notifications about new User models when they are created.
io.socket.get('/user', function gotResponse(body, response) {
  console.log('Current users: ', body);
})
</script>
```

### Realtime communication using low-level methods
Sails在客户端和服务器上暴露出一系列的API来发送自定义的实时消息。

##### 例子
连接一个客户端的socket到服务器，并订阅`hello`事件，接着联系服务器。

```html
<!-- Simply include the sails.io.js script, and a client socket will be created for you -->
<script type"text/javascript" src="/js/dependencies/sails.io.js"></script>
<script type"text/javascript">
// The automatically-created socket is exposed as io.socket.
// Use .on() to subscribe to the 'hello' event on the client
io.socket.on('hello', function gotHelloMessage (data) {
  console.log('Socket `' + data.id + '` joined the party!');
});
// Use .get() to contact the server
io.socket.get('/say/hello', function gotResponse(body, response) {
  console.log('Server responded with status code ' + response.statusCode + ' and data: ', body);
})
</script>
```

在服务器上通过订阅请求socket到"funSockets" room来响应一个`/say/hello`请求，然后广播一个"hello"消息到这个房间的所有的sockets(包括新的那个)。

```javascript
// In /api/controllers/SayController.js
module.exports = {
  hello: function(req, res) {
    // Make sure this is a socket request (not traditional HTTP)
    if (!req.isSocket) {return res.badRequest();}
    // Have the socket which made the request join the "funSockets" room
    sails.sockets.join(req, 'funSockets');
    // Broadcast a "hello" message to all the fun sockets.
    // This message will be sent to all sockets in the "funSockets" room,
    // but will be ignored by any client sockets that are not listening-- i.e. that didn't call `io.socket.on('hello', ...)`
    sails.sockets.broadcast('funSockets', 'hello', req);
    // Respond to the request with an a-ok message
    return res.ok();
  }
}
```

### 参考
+ 参考[sails.io.js library](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)，学习如何在客户端使用sockets与你的Sails app通信。
+ 参考[sails.sockets](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)，学习如何从服务器发送消息到连接上的sockets。
+ 参考[resourceful pub-sub](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，学习如何使用Sails blueprints来自动地发送关于你的[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)改变的实时消息。
+ 访问[Socket.io](http://socket.io/)网站来学习更多关于Sails使用的底层库来实时通信的知识。

<docmeta name="displayName" value="Realtime">
