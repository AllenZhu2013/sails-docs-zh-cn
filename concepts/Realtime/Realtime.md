# 实时通信(aka Sockets)

### 概述
Sails app支持在客户端和服务器间进行全双工的实时通信。这意味着一个客户端(比如一个web浏览器网页或者标签)可以与一个Sails app维护一个永久的连接，并且消息可以在任何时候从客户端发送到服务器或者服务器发送到客户端。实时通信的两个使用场景是即时通信的实现以及多用户游戏。Sails在服务器使用[socket.io](http://socket.io/)库实现实时，在客户端使用[sails.io.js](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)库。在全部的Sails文档中，术语**socket**和**websocket**一般都是用来指在一个Sails app和一个客户端之间的双向、永久的通信通道。

<<<<<<< HEAD
通过sockets与Sails app通信类似于使用AJAX，这两种办法都允许一个网页不用通过刷新就能够与服务器交互。但是，sockets与AJAX有两个重要的不同点：第一，一个socket可以在页面打开之后一直保持连接到服务器，允许它维护*状态*(AJAX请求，像所有的HTTP请求，是*无状态的*)；第二，因为sockets总是自然连接的，所以Sails app能够在任何时候发送数据到一个socket中(因此有"实时"这个绰号)，然而AJAX只允许服务器只有在收到请求的时候才可以做出回应。
=======
Sails apps are capable of full-duplex realtime communication between the client and server.  This means that a client (e.g. browser tab, Raspberry Pi, etc) can maintain a persistent connection to a Sails backend, and messages can be sent from client to server (e.g. AJAX) or from server to client (e.g. "comet") at any time.  Two common uses of realtime communication are live chat implementations and multiplayer games.  Sails implements realtime on the server using the [socket.io](http://socket.io) library, and on the client using the [sails.io.js](http://sailsjs.com/documentation/reference/web-sockets/socket-client/io-socket-on) library.  Throughout the Sails documentation, the terms **socket** and **websocket** are commonly used to refer to a two-way, persistent communication channel between a Sails app and a client.
>>>>>>> upstream/master

### 使用资源丰富的订阅/发布来实时更新模型
产生请求到Sails [blueprints](http://sailsjs.org/documentation/reference/blueprint-api)的sockets会自动地通过[resourceful pub-sub API](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)订阅它们想获取的关于模型的实时消息。你也可以在你自定义的控制器动作中使用这个API来发送消息到对某些模型感兴趣的客户端中。


##### 例子
连接一个客户端的socket到服务器，然后订阅`user`事件，接着请求`/user`订阅到当前和将来的User模型实例。

<<<<<<< HEAD
    ```html
=======
Sockets making requests to Sails' [blueprint actions](http://sailsjs.com/documentation/reference/blueprint-api) are automatically subscribed to realtime messages about the models they retrieve via the [resourceful pub-sub API](http://sailsjs.com/documentation/reference/web-sockets/resourceful-pub-sub).  You can also use this API in your custom controller actions to send out messages to clients interested in certain models.

##### Example

Connect a client-side socket to the server, subscribe to the `user` event, and request `/user` to subscribe to current and future User model instances.

```html
>>>>>>> upstream/master
<!-- Simply include the sails.io.js script, and a client socket will be created for you -->
<script type="text/javascript" src="/js/dependencies/sails.io.js"></script>
<script type="text/javascript">
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

<<<<<<< HEAD
##### 例子
连接一个客户端的socket到服务器，并订阅`hello`事件，接着联系服务器。
=======
Sails exposes a rich API on both the client and the server for sending custom realtime messages.

##### Example

Here's the client-side code to connect a socket to the Sails/Node.js server and listen for an socket event named "hello":
>>>>>>> upstream/master

```html
<!-- Simply include the sails.io.js script, and a client socket will be created and auto-connected for you -->
<script type="text/javascript" src="/js/dependencies/sails.io.js"></script>
<script type="text/javascript">

// The auto-connecting socket is exposed as `io.socket`.

// Use `io.socket.on()` to listen for the 'hello' event:
io.socket.on('hello', function (data) {
  console.log('Socket `' + data.id + '` joined the party!');
});
</script>
```

<<<<<<< HEAD
在服务器上通过订阅请求socket到"funSockets" room来响应一个`/say/hello`请求，然后广播一个"hello"消息到这个房间的所有的sockets(包括新的那个)。
=======
Then, also on the client, we can send a _socket request_.  In this case, we'll wire up the browser to send a socket request when a particular button is clicked:

```js
$('button#say-hello').click(function (){

  // And use `io.socket.get()` to send a request to the server:
  io.socket.get('/say/hello', function gotResponse(data, jwRes) {
    console.log('Server responded with status code ' + jwRes.statusCode + ' and data: ', data);
  });

});

```


Meanwhile, on the server...

To respond to requests to `GET /say/hello`, we use an action.  In our action, we'll subscribe the requesting socket to the "funSockets" room, then broadcast a "hello" message to all sockets in that room (excluding the new one).
>>>>>>> upstream/master

```javascript
// In /api/controllers/SayController.js
module.exports = {

  hello: function(req, res) {

    // Make sure this is a socket request (not traditional HTTP)
    if (!req.isSocket) {
      return res.badRequest();
    }

    // Have the socket which made the request join the "funSockets" room.
    sails.sockets.join(req, 'funSockets');

    // Broadcast a notification to all the sockets who have joined
    // the "funSockets" room, excluding our newly added socket:
    sails.sockets.broadcast('funSockets', 'hello', { howdy: 'hi there!'}, req);

    // ^^^
    // At this point, we've blasted out a socket message to all sockets who have
    // joined the "funSockets" room.  But that doesn't necessarily mean they
    // are _listening_.  In other words, to actually handle the socket message,
    // connected sockets need to be listening for this particular event (in this
    // case, we broadcasted our message with an event name of "hello").  The
    // client-side you'd need to write looks like this:
    // 
    // io.socket.on('hello', function (broadcastedData){
    //   console.log(data.howdy);
    //   // => 'hi there!'
    // }
    // 

    // Now that we've broadcasted our socket message, we still have to continue on
    // with any other logic we need to take care of in our action, and then send a
    // response.  In this case, we're just about wrapped up, so we'll continue on

    // Respond to the request with a 200 OK.
    // The data returned here is what we received back on the client as `data` in:
    // `io.socket.get('/say/hello', function gotResponse(data, jwRes) { /* ... */ });`
    return res.json({
      anyData: 'we want to send back'
    });

  }
}
```

<<<<<<< HEAD
### 参考
+ 参考[sails.io.js library](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)，学习如何在客户端使用sockets与你的Sails app通信。
+ 参考[sails.sockets](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)，学习如何从服务器发送消息到连接上的sockets。
+ 参考[resourceful pub-sub](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，学习如何使用Sails blueprints来自动地发送关于你的[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)改变的实时消息。
+ 访问[Socket.io](http://socket.io/)网站来学习更多关于Sails使用的底层库来实时通信的知识。
=======
### Reference

* See the full reference for the [sails.io.js library](http://sailsjs.com/documentation/reference/web-sockets/socket-client/io-socket-on) to learn how to use sockets on the client side to communicate with your Sails app.
* See the [sails.sockets](http://sailsjs.com/documentation/reference/web-sockets/sails-sockets) reference to learn how to send custom messages from the server to connected sockets.
* See the [resourceful pub-sub](http://sailsjs.com/documentation/reference/web-sockets/resourceful-pub-sub) reference to learn how Sails' blueprint API automatically sends realtime messages about changes to your [models](http://sailsjs.com/documentation/concepts/models-and-orm/models).
* Visit the [Socket.io](http://socket.io) website to learn more about the underlying library Sails uses for realtime communication
>>>>>>> upstream/master

<docmeta name="displayName" value="Realtime">
