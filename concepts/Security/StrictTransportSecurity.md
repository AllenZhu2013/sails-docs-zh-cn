# HTTP严格传输安全
严格传输安全是一个可选的提供安全性能的功能通过使用`HTTPS`而不是`HTTP`。

<<<<<<< HEAD
### 使能STS
实现STS实际上是很容易的，只需要改几行代码([only takes a few lines of code](https://github.com/krakenjs/lusca/blob/master/lib/hsts.js))。但是更好的是，有一些不同的开源模块支持Express和Sails开启这个功能。为了使用它们中的任一个模块，需要从npm中安装，然后打开文件`config/http.js`，并[配置它为一个中间件](http://sailsjs.org/documentation/concepts/Middleware)。下面的这个例子涵盖了基本的使用以及配置。获取更多信息请参考下面的链接。
=======
Strict Transport Security (STS) is an opt-in security enhancement that forces usage of `HTTPS` instead of `HTTP` (in modern browsers, anyway.)

### Enabling STS

Implementing STS is actually very simple and [only takes a few lines of code](https://github.com/krakenjs/lusca/blob/master/lib/hsts.js).  But better yet, a few different open-source modules exist that bring support for this feature to Express and Sails.  To use one of these modules, install it from npm using the directions below, then open `config/http.js` in your project and [configure it as a custom middleware](http://sailsjs.com/documentation/concepts/Middleware).  The example(s) below cover basic usage and configuration.  For more guidance and advanced usage details, be sure and follow the link to the docs.


##### Using [lusca](https://github.com/krakenjs/lusca#luscahstsoptions)

> `lusca` is open-source under the [Apache license](https://github.com/krakenjs/lusca/blob/master/LICENSE.txt)
>>>>>>> upstream/master

##### 使用[lusca](https://github.com/krakenjs/lusca#luscahstsoptions)
> `lusca`是在Apache许可证下的开源软件。

```sh
# In your sails app
npm install lusca --save
```

然后在`config/http.js`文件中配置`middleware`对象：

```js
  // ...
  // maxAge ==> Number of seconds strict transport security will stay in effect.
  strictTransportSecurity: require('lusca').hsts({ maxAge: 31536000 })
  // ...
```

### 额外资料

+ [HTTP Strict Transport Security (OWasp)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security)


<docmeta name="displayName" value="Strict Transport Security">
