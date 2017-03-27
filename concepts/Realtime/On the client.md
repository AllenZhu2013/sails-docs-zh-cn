# 客户端和服务器之间的实时通信
从一个客户端发送一条实时消息到一个Sails app的最简单的方式是使用[sails.io.js](http://sailsjs.org/documentation/reference/web-sockets/sails-io-js)库。这个库允许你简单地连接sockets到一个运行着的Sails app，并提供产生请求到[Sails route](http://sailsjs.org/documentation/concepts/routes)的方法，该路由将以同样的方式作为一个“常规”HTTP请求被处理。

<<<<<<< HEAD
sails.io.js库使用一个`<script>`标签自动地添加到新的Sails app中的默认[布局模板](http://sailsjs.org/documentation/concepts/views/layouts)。当一个web页面加载`sails.io.js`脚本，它尝试创建一个[客户端socket](http://sailsjs.org/documentation/reference/web-sockets/socket-client/sails-socket)并连接它到Sails app，然后将其暴露为全局变量`io.socket`。

### 例子
包含sails.io.js库文件，并使用自动连接的socket产生一条请求到Sails app的/hello路由：
=======
The easiest way to send a realtime message from a client to a Sails app is by using the [sails.io.js](http://sailsjs.com/documentation/reference/web-sockets/sails-io-js) library.  This library allows you to easily connect sockets to a running Sails app, and provides methods for making requests to [Sails routes](http://sailsjs.com/documentation/concepts/routes) that are handled in the same manner as a "regular" HTTP request.

The sails.io.js library is automatically added to the default [layout template](http://sailsjs.com/documentation/concepts/views/layouts) of new Sails apps using a `<script>` tag.  When a web page loads the `sails.io.js` script, it attempts to create a new [client socket](http://sailsjs.com/documentation/reference/web-sockets/socket-client/sails-socket) and connect it to the Sails app, exposing it as the global variable `io.socket`.

### Examples

Include the `sails.io.js` library, and make a request to the `/hello` route of a Sails app using the automatically-connected socket:
>>>>>>> upstream/master

```html
<script type="text/javascript" src="/js/dependencies/sails.io.js"></script>
<script type="text/javascript">
io.socket.get('/hello', function responseFromServer (body, response) {
  console.log("The server responded with status " + response.statusCode + " and said: ", body);
});
</script>
```

手工创建一个新的客户端并记录消息日志当它连接到服务器的时候：

```html
<script type="text/javascript" src="/js/dependencies/sails.io.js" autoConnect="false"></script>
<script type="text/javascript">
var mySocket = io.sails.connect();
mySocket.on('connect', function onConnect () {
  console.log("Socket connected!");
});
</script>
```

<<<<<<< HEAD
### Socket requests vs AJAX requests
你也许已经注意到一个客户端socket的`.get()`与产生一条AJAX请求很类似，比如通过使用jQuery的`$.get()`方法。这么做事故意的--因为我们的目的就是无论这个请求从哪里生成都能够从Sails中得到相同的响应。使用一个客户端的socket产生请求的好处是在Sails app中的[控制器动作](http://sailsjs.org/documentation/concepts/controllers#?actions)将有权限访问到产生该请求的socket，允许它去订阅那个socket的实时通知(参考[从服务器发送实时消息](http://sailsjs.org/documentation/concepts/realtime/sending-realtime-messages-from-the-server-to-one-or-more-clients))。
=======
### Socket requests vs traditional AJAX requests

You may have noticed that a client socket `.get()` is very similar to making an AJAX request, for example by using jQuery's `$.get()` method.  This is intentional&mdash;the goal is for you to be able to get the same response from Sails no matter where the request originated from.  The benefit to making the request using a client socket is that the [controller action](http://sailsjs.com/documentation/concepts/controllers#?actions) in your Sails app will have access to the socket which made the request, allowing it to _subscribe_ that socket to realtime notifications (see [sending realtime messages from the server](http://sailsjs.com/documentation/concepts/realtime/on-the-server)).
>>>>>>> upstream/master


<<<<<<< HEAD
### 参考
+ 参考[sails.io.js library](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)，学习如何在客户端使用sockets与你的Sails app通信。
+ 参考[sails.sockets](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)，学习如何从服务器发送消息到连接上的sockets。
+ 参考[resourceful pub-sub](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，学习如何使用Sails blueprints来自动地发送关于你的[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)改变的实时消息。
+ 访问[Socket.io](http://socket.io/)网站来学习更多关于Sails使用的底层库来实时通信的知识。
=======
* View the full [sails.io.js library](http://sailsjs.com/documentation/reference/web-sockets/socket-client) reference.
* See the [sails.sockets](http://sailsjs.com/documentation/reference/web-sockets/sails-sockets) reference to learn how to send messages from the server to connected sockets
* See the [resourceful pub-sub](http://sailsjs.com/documentation/reference/web-sockets/resourceful-pub-sub) reference to learn how to use Sails blueprints to automatically send realtime messages about changes to your [models](http://sailsjs.com/documentation/concepts/models-and-orm/models).
* Visit the [Socket.io](http://socket.io) website to learn more about the underlying library Sails uses for realtime communication
>>>>>>> upstream/master

<docmeta name="displayName" value="On the client">
