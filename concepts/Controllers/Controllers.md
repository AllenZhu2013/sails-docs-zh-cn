# Controllers
### 概述
在你的Sails app中控制器(**MVC**中的**C**)是最主要的对象，它负责回复从web浏览器后者移动设备或者其他能够与服务器沟通的系统发出的请求。控制器经常被作为你的[模型](http://sailsjs.org/documentation/concepts/ORM/Models.html)和[视图](http://sailsjs.org/documentation/concepts/Views)之间的中间人。对于绝大部分应用，控制器将包含你的项目中一整块的[业务逻辑](http://en.wikipedia.org/wiki/Business_logic)。

### 动作
控制器包含一组称为==actions==的函数。在你的应用中action可以绑定到[路由](http://sailsjs.org/documentation/concepts/Routes)中所以当一个客户请求这条路由的时候，绑定的方法将被执行以完成一些业务逻辑并生成一条回应消息。比如，`GET/hello`路由可以绑定如下的一个方法在你的应用中：

```javascript
function (req, res) {
  return res.send("Hi there!");
}
```

所以任何时候一个web浏览器指向`/hello`的URL到你的app服务器，该页面会显示消息“Hi there”。

### 控制器在哪里定义？
控制器都是定义在`api/controllers/`文件夹下。你可以放任何你想要放的文件到这个目录下，但是为了能让Sails将它们加载为控制器，文件的名称都应该以`Controller.js`结尾。为了方便，Sails控制器都是[Pascal-cased](http://c2.com/cgi/wiki?PascalCase)，所以文件名的每个单词(包括第一个单词)都是大写的：比如，`UserController.js`, `MyController.js`和 `SomeGreatBigController.js`都是有效的Pascal-cased名称。

你也许会组织你的控制器成组然后保存在`api/controllers`的子文件夹下，然而注意的是当用于路由的时候子文件夹的名字*将会成为控制器身份识别的一部分*(更多请参考Route一章)

### 控制器的文件内容应该是什么？
一个控制器的文件定义成一个JS对象，其中关键词是action的名字，值是对应的action方法。以下是一个简单控制器的例子：

```javascript
module.exports = {
  hi: function (req, res) {
    return res.send("Hi there!");
  },
  bye: function (req, res) {
    return res.redirect("http://www.sayonara.com");
  }
};
```

该控制器定义了两个动作：“hi”回应给请求一组字符串消息，但是“bye”动作的回应是直接重定向到别的网站。对于使用Express.js写过服务器的人都很熟悉`req`和`res`。因为Sails本来就是使用的[Express.js](https://github.com/expressjs)来处理路由，所以设计成这样是意料之内的。需要特别注意的是，action中是缺少一个`next`参数的。不像Express中间件的方法，Sails控制器的动作应该总是成为请求链中的终点站，也就是说控制器应该以一个回应或者一个错误来结束请求。虽然有可能在动作中使用`next`，但是在任何可能的情况下我们还是强烈鼓励你使用[policies](http://sailsjs.org/documentation/concepts/Policies)。

### “瘦小“的控制器
绝大部分的MVC框架都是建议写出一个”瘦小精简“的控制器，Sails也不例外(让你的Sails控制器越简单越好是个好主意的)。

控制器的代码内在地依赖于某种触发器或者事件。在一个后端框架中如Sails，这种事件大部分是入口的请求。所以如果你在你的控制器的action中写了一大堆代码，那么那段代码的作用域就依赖于”请求的上下文“(`req`和`res`对象)便不再少见。这看着貌似没啥不好...直到你想要从一个有些许差别的动作中或者从命令行中使用那段代码。

所以”精简控制器“的本质是鼓励从那些相关作用于交错复杂的代码中解耦出可重用的代码。在Sails中，你可以通过一些方法来践行这个准则，但是绝大部分的策略都是为了从控制器中外推出代码：

+ 写出一个通用的模型方法来封装一些代码，这些代码执行一个与特殊模型相关的特殊任务。
+ 写出一个service作为一段函数来封装一些代码，该段代码执行一个特殊的专用应用的任务。
+ 如果你发现一段有用的代码可以跨应用，你应该将其抽出来作为一个node模块。然后你可以将其在你的组织中共享，将其使用到未来的项目中，或者更好的做法是以一种开源的许可证允许的形式将其[发布到npm平台](https://docs.npmjs.com/getting-started/publishing-npm-packages)，这样别的开发者便可以使用和维护。



<docmeta name="displayName" value="Controllers">
