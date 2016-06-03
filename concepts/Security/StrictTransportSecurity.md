# HTTP严格传输安全
严格传输安全是一个可选的提供安全性能的功能通过使用`HTTPS`而不是`HTTP`。

### 使能STS
实现STS实际上是很容易的，只需要改几行代码([only takes a few lines of code](https://github.com/krakenjs/lusca/blob/master/lib/hsts.js))。但是更好的是，有一些不同的开源模块支持Express和Sails开启这个功能。为了使用它们中的任一个模块，需要从npm中安装，然后打开文件`config/http.js`，并[配置它为一个中间件](http://sailsjs.org/documentation/concepts/Middleware)。下面的这个例子涵盖了基本的使用以及配置。获取更多信息请参考下面的链接。

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
