# CORS(Cross-Origin Resource Sharing)

<!--
Your default Sails setup is already equipped to handle AJAX requests from a web page on the same domain.  But what if you need to handle AJAX requests originating from other domains?  You could set up your browser JSONP That's where [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) comes in.
-->

[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)是一种允许你的服务器(比如 api.mysite.com)的页面上的浏览器脚本给别的域名使用(比如myothersite.com)的机制。就像JSONP，CORS的目的是一种功能，该功能作为一种安全的方法来避免[同源策略](http://en.wikipedia.org/wiki/Same-origin_policy)；允许你的sails服务器成功地回应来自其他域名商运行的客户端JS代码的请求。但是不想JSONP那样，它不仅仅工作于GET请求。

Sails可以被配置成允许从你指定的一组域名列表中跨域请求，或者从每个域名中跨域请求。在你的app这个可以在一条预路由基础上或者全局每一条路由上实现。

### 使能CORS
因为安全的原因，Sails默认关闭CORS，但是使能它也是很简单地事情。

为了在你的app上允许从任何域到任何路由跨域请求，只需要在[`config/cors.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.cors.html)中使能`allRoutes`：

```javascript
allRoutes: true
```

参考[`sails.config.cors`](http://sailsjs.org/documentation/reference/sails.config/sails.config.cors.html)以了解更多的所有可用选项的全面参考。

### 给一条路由配置CORS
除了全局的CORS配置，你可以在`config/routes.js`中设置单独的一条路由去接受或者拒绝跨域请求。在`config/cors.js`中为了指示某条路由使用配置参数应该接受CORS请求，设置`cors`属性为`true`：

```javascript
"get /foo": {
   controller: "FooController",
   action: "index",
   cors: true
}
```

如果你在`config.cors.js`文件中将`allRoutes`参数设置为`true`，但是你想去除某条指定的路由，那么你可以明确地设置它的`cors`参数为`false`：

```javascript
"get /foo": {
   controller: "FooController",
   action: "index",
   cors: false
}
```

为了覆盖一条路由专用的CORS配置参数，可以添加一个`cors`属性对象：

```javascript
"get /foo": {
   controller: "FooController",
   action: "index",
   cors: {
     origin: "http://sailsjs.org, http://sailsjs.com",
     credentials: false
   }
}
```

### 安全级别
默认Sails会忽略域名而处理所有的入口请求，甚至当CORS使能的时候：Sails会简单地在响应中设置合适的头部然后客户端才可以决定是否显示这条响应。比如：如果你从一个域中发送一条`GET`请求到`/foo/bar`，并且该域不再你的CORS白名单中，在你的`FooController.js`文件中的`bar`动作仍然会执行，但是浏览器将会丢掉这个结果。这个看起来有点违反常规思维，但是却很重要因为它允许那些基于非浏览器的客户端(比如[Postman](https://www.getpostman.com)或[curl](http://curl.haxx.se/))正常工作，可以防止那些同域策略类型的攻击。

如果你想要完全地保护Sails不处理那些不允许的域发出的请求，你可以设置`securityLevel`：

```javascript
module.exports.cors = {
  allRoutes: true,
  origin: "http://sailsjs.org",
  securityLevel: 1
}
```

安全级别1(high)将会响应一个403状态码给任何不允许跨域的带有`http`或`https`协议前缀的请求。安全级别2(very high)也会这么做，但是它会扩展到所有的协议(所以像Postman和curl将不会正常工作)。


<docmeta name="displayName" value="CORS">
