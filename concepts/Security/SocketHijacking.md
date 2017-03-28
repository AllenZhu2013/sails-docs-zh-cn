# 套接字劫持
不幸的是，跨站伪造请求攻击不限于HTTP协议。在大部分的实时应用中套接字劫持(也就是众所周知的[CSWSH](http://www.christian-schneider.net/CrossSiteWebSocketHijacking.html))是一个容易让人忽略的乳垫。幸运的是，因为Sails将HTTP和WebSocket请求视为头等请求，所以内建的[CSRF protection](http://sailsjs.org/documentation/concepts/Security/CSRF.html)和[configurable CORS rulesets](http://sailsjs.org/documentation/concepts/Security/CORS.html)都可以应用到这两个协议上。

你可以通过使能内建的[`config/csrf.js`](http://sailsjs.org/documentation/anatomy/myApp/config/csrf.js.html) 保护来阻止CSWSH攻击并确保`_csrf`令牌和所有的入口socket请求一起发送。另外，如果你计划允许跨域套接字连接到你的服务器，那么你需要配置你的CORS设置。你也可以在[`config/sockets.js`](http://sailsjs.org/documentation/anatomy/myApp/config/sockets.js.html)定义`authorization`设置作为一个自定义的功能以允许或者拒绝初始化的套接字连接基于你的需求。


#### 注意：

+ CSWSH保护只在人们使用相同的客户端应用程序来套接字连接多个web应用的场景下才值得关心一下。





<docmeta name="displayName" value="Socket Hijacking">
