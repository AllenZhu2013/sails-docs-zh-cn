# 常见的默认值
Sails有绑定一组默认的常用的HTTP中间件供你使用。当然你可以禁用、重写或者追加新的内容，但是对于在产品或者开发模式下大部分的app的预安装的栈是完全可以接受的。下面是一组标准的HTTP中间件函数，有序地绑定到Sails中，当每次有HTTP收到的时候便会执行：

HTTP Middleware Key       | Purpose
 :------------------------ |:------------
 **startRequestTimer**     | 当请求开始的时候在内存中分配一个变量保存时间戳。这个可以被你的app访问到并被用于提供关于慢请求的诊断信息。
 _cookieParser_ *          | Parses the cookie header into a clean object for use in subsequent middleware and your application code.
 _session_ *               | Sets up a unique session object using your [session configuration](http://sailsjs.org/documentation/reference/sails.config/sails.config.session.html).
 **bodyParser**            | Parses parameters and binary upstreams (for streaming file uploads) from the HTTP request body using [Skipper](https://github.com/balderdashy/skipper).
 **compress**              | Compresses response data using gzip/deflate. See [`compression`](https://github.com/expressjs/compression) for details.
 **methodOverride**        | Provides faux HTTP method support, letting you use HTTP verbs such as PUT or DELETE in places where the client doesn't support it (e.g. legacy versions of Internet Explorer.)  If a request has a `_method` parameter set to `"PUT"`, the request will be routed as if it was a proper PUT request.  See [Connect's methodOverride docs](http://www.senchalabs.org/connect/methodOverride.html) for more information if you need it.
 **poweredBy**             | Attaches an `X-Powered-By` header to outgoing responses.
 **$custom**               | Provides backwards compatibility for a configuration option from Sails v0.9.x.  Since Sails v0.10 offers much more configuration flexibility for HTTP middleware, as long as you are not using `sails.config.express.customMiddleware`, you can confidently remove this item from the list.
 _router_ *                | This is where the bulk of your app logic gets applied to any given request.  In addition to running `"before"` handlers in hooks (e.g. csrf token enforcement) and some internal Sails logic, this routes requests using your app's explicit routes (in [`sails.config.routes`](http://sailsjs.org/documentation/reference/sails.config/sails.config.routes.html)) and/or route blueprints.
 _www_ *                   | Serves static files- usually images, stylesheets, scripts- in your app's "public" folder (configured in [`sails.config.paths`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md), conventionally [`.tmp/public/`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)) using Connect's [static middleware](http://www.senchalabs.org/connect/static.html).
 **favicon**               | Serves the [browser favicon](http://en.wikipedia.org/wiki/Favicon) for your app if one is provided as `/assets/favicon.ico`.
 _404_ *                   | Handles requests which do not match any routes - triggers `res.notFound()`  <!-- technically, this emits the `router:request:404` event)  -->
 _500_ *                   | Handles requests which trigger an internal error (i.e. call Express's `next(err)`)  - triggers `res.serverError()` <!-- technically, this emits the `router:request:500` event)  -->

<docmeta name="displayName" value="Conventional Defaults">
