# P3P
### 背景
P3P表示的是个人隐私安全平台项目，是一种web/浏览器标准，被设计成获得更好的用户web隐私控制。当前(截止2014年)，所有的主流浏览器中只有IE浏览器支持。它通常发挥作用在处理老旧的应用程序上。

许多现代的组织都执意地忽略P3P。下面是[Facebook](https://www.facebook.com/help/327993273962160/)关于该主题说过的话：

> 建立P3P的组织叫做Wide Web Consortium，它因为大部分的现代浏览器都没有完全支持P3P所以几年前暂停了他们的工作。因为，P3P的标准现在有点过时并且也不会体现在当前web使用的技术上，所以大部分的网站都没有P3P策略。
> 其他信息也可以参考http://www.zdnet.com/blog/facebook/facebook-to-microsoft-p3p-is-outdated-what-else-ya-got/9332


### Sails支持的P3P
但是抛开这些不谈，有时候你无论如何都不得不实现P3P。

幸运的是，有一些不同的模块存在支持Express或Sails的P3P功能只需要在相关的P3P头部使能。为了使用这些模块来处理P3P头部，使用下面的的指令从npm中安装，然后在你的项目中打开`config/http.js`文件并将它配置为一个自动以的中间件。要做到这一点，定义你的P3P中间件为“p3p"，并且添加字符串”p3p"到你想要在中间链中运行的`middleware.order`数组中(放置最好的地方是在`cookieParser`的前面)。

在`config/http.js`中的例子：

```js
// .....
module.exports.http = {

  middleware: {

    p3p: require('p3p')(p3p.recommmended), // <==== set up the custom middleware here and named it "p3p"

    order: [
      'startRequestTimer',
      'p3p', // <============ configured the order of our "p3p" custom middleware here
      'cookieParser',
      'session',
      'bodyParser',
      'handleBodyParserError',
      'compress',
      'methodOverride',
      'poweredBy',
      '$custom',
      'router',
      'www',
      'favicon',
      '404',
      '500'
    ],
    // .....
  }
};
```

参考下面的例子获取更多细节-并确保跟随你正在使用的模块的文档链接，获取最新的信息，它的特性的对比分析以及任何相关的bug修复和高级使用细节。

##### Using [node-p3p](https://github.com/troygoode/node-p3p)
> `node-p3p`是在MIT许可证下的开源项目。

```sh
# In your sails app
npm install p3p --save
```

然后在`config/http.js`的`middleware`配置对象中配置：

```js
  // ...
  // node-p3p provides a recommended compact privacy policy out of the box
  p3p: require('p3p')(require('p3p').recommended)
  // ...
```


##### Using [lusca](https://github.com/krakenjs/lusca#luscap3pvalue)
> `lusca`是在MIT许可证下的开源项目。

```sh
# In your sails app
npm install lusca --save
```

然后在`config/http.j`s的`middleware`配置对象中配置：

```js
  // ...
  // "ABCDEF" ==> The compact privacy policy to use.
  p3p: require('lusca').p3p('ABCDEF')
  // ...
```

### 额外资源：

+ [Description of the P3P Project (Microsoft)](http://support.microsoft.com/kb/290333)
+ ["P3P Work suspended" (W3C)](http://www.w3.org/P3P/)
+ [P3P Compact Specification (CompactPrivacyPolicy.org)](http://compactprivacypolicy.org/compact_specification.htm)


<docmeta name="displayName" value="P3P">

