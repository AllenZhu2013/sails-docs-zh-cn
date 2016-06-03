# 在多服务器(也就是"集群")环境下的实时通信
默认的配置下，Sails允许一个服务器与所有连接上的客户端进行实时通信。当[scaling your Sails app to multiple servers](http://sailsjs.org/documentation/concepts/deployment/scaling)，需要设置一些额外的配置，这些设置是为了实时消息可以可靠地传递到客户端而忽略它们连接的哪一台服务器。这些设置概括起来是：

1. 设置一个[hosted](https://www.google.com/search?q=hosted+redis)的[Redis](http://redis.io/)实例。
2. 安装[socket.io-redis](https://github.com/socketio/socket.io-redis)，将其作为你的Sails app的一个依赖库
3. 更新你的[sails.config.sockets.adapter ](http://sailsjs.org/documentation/reference/configuration/sails-config-sockets#?commonlyused-options)设置为`socket.io-redis`并且设置指向你的hosted Redis实例的合适的`主机`、`密码`等字段。

在你的hosted Redis安装中不需要特殊的设置；只需要插入合适的主机地址和证书到你的`/config/sockets.js`文件然后`socket.io-redis`适配器将会为你包办剩下的一切工作。

> 注意：当在多服务器环境下操作的时候，某些操作需要一个不确定的时间才能完成，即使代码似乎是立即执行。需要牢记一点的是当考虑代码将会，比如调用[.broadcast()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/sails-sockets-broadcast)的时候会立即接着调用[.addRoomMembersToRoom()](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets/add-room-members-to-room)。在这种情况下，你也许想要使用一些方法比如通过将其封装到一个`setTimeout()`延迟广播调用来给在集群时间中每个服务器响应。

### 参考
+ 参考[sails.io.js library](http://sailsjs.org/documentation/reference/web-sockets/socket-client/io-socket-on)，学习如何在客户端使用sockets与你的Sails app通信。
+ 参考[sails.sockets](http://sailsjs.org/documentation/reference/web-sockets/sails-sockets)，学习如何从服务器发送消息到连接上的sockets。
+ 参考[resourceful pub-sub](http://sailsjs.org/documentation/reference/web-sockets/resourceful-pub-sub)，学习如何使用Sails blueprints来自动地发送关于你的[模型](http://sailsjs.org/documentation/concepts/models-and-orm/models)改变的实时消息。
+ 访问[Socket.io](http://socket.io/)网站来学习更多关于Sails使用的底层库来实时通信的知识。

<docmeta name="displayName" value="Multi-server environments">
