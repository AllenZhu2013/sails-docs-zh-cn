# 可编程地使用Sails
### 概述
大部分时间，你通过Sails的[命令行接口](http://sailsjs.org/documentation/reference/command-line-interface)与Sails交互，使用[sails lift](http://sailsjs.org/documentation/reference/command-line-interface/sails-lift)来启动服务器。Sails app其实还可以通过其它的Node apps使用[可编程的接口](http://sailsjs.org/documentation/reference/application)来启动和操作。该接口最主要的用途就是在自动化测试组件中运行Sails的app。

### 可编程地创建一个Sails app
为了在一个Node.js脚本中创建一个新的Sails app，你可以使用Sails的*constructor*。你可以使用相同的构造器去创建你想要的各种不同的Sails app。

```javascript
var Sails = require('sails').constructor;
var mySailsApp = new Sails();
var myOtherSailsApp = new Sails();
```


### 可编程地配置、启动、结束Sails app
一旦你有了一个Sails app的引用之后，你可以使用[`.load()`](http://sailsjs.org/documentation/reference/application/sails-load)或者[`lift()`](http://sailsjs.org/documentation/reference/application/sails-lift)来启动Sails。这两种方法都带有两个参数：配置选项的目录，以及一个回调函数，该回调函数会在Sails启动之后调用。

 > 当Sails可编程地启动之后，它仍然会使用`api`,`config`和在当前工作目录下的其他文件夹来加载控制器、模型和配置选项。一个值得注意的问题是当以这种方式启动app的时候`.sailsrc`文件不会被加载。

> 那些作为参数发送给`.load()`或`.lift()`的配置选项的优先级都高于从其他任何地方加载的配置选项。

`.load()`和`.lift()`不同的地方在于.lift()会多执行额外的几个步骤：(1)运行app的[bootstrap](http://sailsjs.org/documentation/reference/configuration/sails-config-bootstrap)(如果有的话)，(2)启动一个HTTP服务器并监听通过`sails.config.port`配置的端口号(默认是1337).这就允许你产生一个HTTP请求来启动app。为了产生一个请求到app中以`.load()`的方式启动，你可以使用[.request()](http://sailsjs.org/documentation/reference/application/sails-request)的方法来加载app。

 ```javascript
// Starting an app with .lift() on port 1338 and making a POST request
mySailsApp.lift({port: 1338}, function(err, theApp) {
  // If an error occurred lifting the app, exit
  if (err) {
    return console.error("An error occurred: ", err);
  }
  // Make a request using the "request" library and display the response.
  // Note that you still must have an `api/controllers/FooController.js` file
  // under the current working directory, or a `/foo` or `POST /foo` route
  // set up in `config/routes.js`.
  require('request').post("/foo", console.log);
});

// Starting an app with .load() and making the same POST request
mySailsApp.load({}, function(err, theApp) {
  // If an error occurred lifting the app, exit
  if (err) {
    return console.error("An error occurred: ", err);
  }
  // Make a request using the "request" method and display the response.
  // Note that you still must have an `api/controllers/FooController.js` file
  // under the current working directory, or a `/foo` or `POST /foo` route
  // set up in `config/routes.js`.
  theApp.request({url:"/foo", method: "post"}, console.log);
});
```

关掉一个app的方法是使用`.lower()`：

```javascript
mySailsApp.lower(function(err) {
  if (err) {
     console.log("An error occured while stopping app: ", err);
  }
});
```

### 参考
Sails可编程接口的完整参考文档在http://sailsjs.org/documentation/reference/application中可以查看到。

<docmeta name="displayName" value="Programmatic Usage">
