# Internationalization
### 概述
如果你的app会在全世界的范围内被人们或者系统访问到，那么国际化和本地化(i18n)将会是你的国际化策略非常重要的一部分。Sails内建了支持检测用户的语言选项并使用[i18n-node](https://github.com/mashpie/i18n-node)翻译静态的词句。([npm](https://www.npmjs.org/package/i18n))

<!--
  Potentially cover this:
  *(but it might be obvious and not useful/necessary to include, not sure- could also be more confusing than helpful)*
Note that this built-in support is for **dynamically-rendered** (but otherwise **static**) content.  You can only use it in responses which are pre-processed on the server.  In other words, you can use these translations in your views, controller actions, and policies, but stuff in your assets folder.)

we do not recommend translating strings in the front-end of your application (e.g. the browser or an iOS app) for a variety of reasons, the most obvious being SEO, but also fragmentation. You can of course still do so- just don't use this built-in support from the i18n hook.
-->

### 使用方法
在一个视图中：

```ejs
<h1> <%= __('Hello') %> </h1>
<h1> <%= __('Hello %s, how are you today?', 'Mike') %> </h1>
<p> <%= i18n('That\'s right-- you can use either i18n() or __()') %> </p>
```
在控制器或者策略中：

```javascript
req.__('Hello'); // => Hola
req.__('Hello %s', 'Marcus'); // => Hola Marcus
req.__('Hello {{name}}', { name: 'Marcus' }); // => Hola Marcus
```

或者如果你知道本地化的id，你可以使用`sails._`翻译任何语句：

```javascript
sails.__({
  phrase: 'Hello',
  locale: 'es'
});
// => 'Hola!'
```

### 额外的选项
国际化和本地化的配置是在[`sails.config.i18n`](http://sailsjs.org/documentation/reference/sails.config/sails.config.i18n.html)文件中。你会去修改这些配置的最普遍原因是你需要修改你支持的地方的列表或者你想翻译的文件存储的位置。

```javascript
// Which locales are supported?
locales: ['en', 'es'],

// Where are your locale translations located?
localesDirectory: '/config/locales'
```

### 禁用或者自定义Sails的默认国际化支持
当然你可以总是`require()`你喜欢的任何Node模块在你的工程的任何地方，也可以使用你想要的国际化策略。

但是值得注意的是因为Sails实现[node-i18n](https://github.com/mashpie/i18n-node)集成到[i18n的钩子](http://sailsjs.org/documentation/concepts/Internationalization)中，所以你可以使用[`loadHooks`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)或者[`hooks`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)配置选项完全禁用或者重写它。

### 在客户端的i18n如何？
上面的技术在服务器端的视图中工作得很好，但是如果客户端的app使用从一个CDN或者静态主机获取的HTML模板的时候又该怎样呢？(比如专注于性能的SPAs或者PhoneGap apps或者Chrome扩展)。

你实际可以重用Sails的i18n来支持你获取翻译的模板到浏览器。如果你想要使用Sails去国际化你的客户端模板，那么把你的前端模板放在你的app下的`/views`目录下。

+ 在开发模式下，你应该每次重翻译和预编译你的模板，使用grunt-contrib-watch来监控相关的stringfile或者模板文件的改动，该任务已经默认安装在Sails的项目中；
+ 在产品模式下，你想要在Sails运行起来的时候翻译和预编译所有的模板。在加载时间严格要求的场景下(比如移动设备app)，你甚至可以上传你的已经翻译好并预编译压缩的模板到CDN比如Cloudfront以提高服务器的性能。


<docmeta name="displayName" value="Internationalization">
