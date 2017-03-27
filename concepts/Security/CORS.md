# CORS(Cross-Origin Resource Sharing)

<!--
Every Sails app comes ready to handle AJAX requests from a web page on the same domain.  But what if you need to handle AJAX requests 
originating from other domains?
-->

<<<<<<< HEAD
[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)是一种允许你的服务器(比如 api.mysite.com)的页面上的浏览器脚本给别的域名使用(比如myothersite.com)的机制。就像JSONP，CORS的目的是一种功能，该功能作为一种安全的方法来避免[同源策略](http://en.wikipedia.org/wiki/Same-origin_policy)；允许你的sails服务器成功地回应来自其他域名商运行的客户端JS代码的请求。但是不想JSONP那样，它不仅仅工作于GET请求。
=======
[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a mechanism that allows browser scripts on pages served from other domains (e.g. myothersite.com) to talk to your server (e.g. api.mysite.com).  Like [JSONP](https://en.wikipedia.org/wiki/JSONP), the goal of CORS is to circumvent the [same-origin policy](http://en.wikipedia.org/wiki/Same-origin_policy); allowing your Sails server to successfully respond to requests from client-side JavaScript code running on a page hosted from some other domain.  But unlike JSONP, it works with more than just GET requests.  And it allows you to whitelist particular origins (`staging.yoursite.com` or `yourothersite.net`) and prevent requests from others (`evil.com`).
>>>>>>> upstream/master

Sails可以被配置成允许从你指定的一组域名列表中跨域请求，或者从每个域名中跨域请求。在你的app这个可以在一条预路由基础上或者全局每一条路由上实现。

<<<<<<< HEAD
### 使能CORS
因为安全的原因，Sails默认关闭CORS，但是使能它也是很简单地事情。

为了在你的app上允许从任何域到任何路由跨域请求，只需要在[`config/cors.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.cors.html)中使能`allRoutes`：
=======
### Enabling CORS

For security reasons, CORS is disabled by default in Sails.  But enabling it is dead-simple.

To allow cross-origin requests from a whitelist of trusted domains to _any_ route in your app, simply enable `allRoutes` and provide an `origin` setting in [`config/cors.js`](http://sailsjs.com/docs/reference/configuration/sails-config-cors):
>>>>>>> upstream/master

```javascript
allRoutes: true,
origin: 'example.com,api.example.com,blog.example.com,foo.com'
```

<<<<<<< HEAD
参考[`sails.config.cors`](http://sailsjs.org/documentation/reference/sails.config/sails.config.cors.html)以了解更多的所有可用选项的全面参考。

### 给一条路由配置CORS
除了全局的CORS配置，你可以在`config/routes.js`中设置单独的一条路由去接受或者拒绝跨域请求。在`config/cors.js`中为了指示某条路由使用配置参数应该接受CORS请求，设置`cors`属性为`true`：
=======
To allow cross-origin requests from _any_ domain to _any_ route in your app, use `origin: '*'`:
>>>>>>> upstream/master

```javascript
allRoutes: true,
origin: '*',
credentials: false
```

<<<<<<< HEAD
如果你在`config.cors.js`文件中将`allRoutes`参数设置为`true`，但是你想去除某条指定的路由，那么你可以明确地设置它的`cors`参数为`false`：
=======
> #### WARNING:
> If you enable CORS with `origin: '*'`, but fail to also set `credentials: false`, your app will **not** be protected against attacks that exploit CORS.  To prevent third-party sites from being able to trick your logged-in users into making unauthorized requests to your app, you should either (A) set `origin` to a specific set of trusted domains, or (B) keep `origin: '*'` but set `credentials: false`.  Just realize that, if you choose to set `credentials: false`, affected routes will not be able to access the [session](http://sailsjs.com/docs/concepts/sessions).


See [`sails.config.cors`](http://sailsjs.com/docs/reference/configuration/sails-config-cors) for a comprehensive reference of all available options.


### Configuring CORS For individual routes
Besides the global CORS configuration in `config/cors.js`, you can also configure these settings on a per-route basis in [`config/routes.js`](http://sailsjs.com/anatomy/config/routes-js).

If you set `allRoutes: true` in `config/cors.js`, but you want to exempt a specific route, set the `cors: false` in the route's target:
>>>>>>> upstream/master

```javascript
'POST /signup': {
   controller: 'UserController',
   action: 'signup',
   cors: false
}
```

<<<<<<< HEAD
为了覆盖一条路由专用的CORS配置参数，可以添加一个`cors`属性对象：
=======
To enable or override global CORS configuration for a particular route, provide `cors` as a dictionary:
>>>>>>> upstream/master

```javascript
'GET /videos': {
   controller: 'VideoController',
   action: 'find',
   cors: {
     origin: 'example.com,api.example.com,blog.example.com,foo.com',
     credentials: false
   }
}
```

<<<<<<< HEAD
### 安全级别
默认Sails会忽略域名而处理所有的入口请求，甚至当CORS使能的时候：Sails会简单地在响应中设置合适的头部然后客户端才可以决定是否显示这条响应。比如：如果你从一个域中发送一条`GET`请求到`/foo/bar`，并且该域不再你的CORS白名单中，在你的`FooController.js`文件中的`bar`动作仍然会执行，但是浏览器将会丢掉这个结果。这个看起来有点违反常规思维，但是却很重要因为它允许那些基于非浏览器的客户端(比如[Postman](https://www.getpostman.com)或[curl](http://curl.haxx.se/))正常工作，可以防止那些同域策略类型的攻击。
=======

### Security levels

The browser's cross-origin policy allows requests to be sent-- it simply does not allow the browser to receive the response.  Similarly, Sails will still process all the requests that come in regardless of domain, even with CORS enabled.  But for routes with CORS enabled, it will simply set the appropriate headers on the response so that the *browser* can decide whether or not to expose the response.  For example, if you send a `GET` request to `/foo/bar` from a domain that is not in your CORS whitelist (`origin`), the `bar` action in your `FooController.js` file will still run, but the browser will throw away the result.  This may seem counterintuitive, but it is important because it allows non-browser-based clients (like [Postman](https://www.getpostman.com) and [curl](http://curl.haxx.se/)) to work while still blocking the kind of attacks that the [Same-Origin Policy](http://en.wikipedia.org/wiki/Same-origin_policy) is meant to protect against.
>>>>>>> upstream/master

如果你想要完全地保护Sails不处理那些不允许的域发出的请求，你可以设置`securityLevel`：

```javascript
module.exports.cors = {
  allRoutes: true,
  origin: "http://sailsjs.com",
  securityLevel: 1
}
```

安全级别1(high)将会响应一个403状态码给任何不允许跨域的带有`http`或`https`协议前缀的请求。安全级别2(very high)也会这么做，但是它会扩展到所有的协议(所以像Postman和curl将不会正常工作)。


### Notes
 
> + CORS is not supported in Internet Explorer 7.  Fortunately, it is supported in IE8 and up, as well as in all other modern browsers.
> + Read [more about CORS from MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
> + Read the [CORS spec](https://www.w3.org/TR/cors/)

<docmeta name="displayName" value="CORS">
