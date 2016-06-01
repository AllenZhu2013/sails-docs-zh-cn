# Assets

### 概述
Assets指的是在你的服务器上想展示给外部世界看的[静态文件](http://en.wikipedia.org/wiki/Static_web_page)(js,css,images等。在Sails中，这些文件被放置在[assets/](http://sailsjs.org/documentation/anatomy/myApp/assets)目录下，当你运行起服务器后，该目录下的所有文件将会被处理(译者注：指的是编译、压缩等操作)然后与一个隐藏的目录(`./tmp/public/`)保持同步。`./tmp/public`文件夹下的内容才是Sails实际展示给外部的，类似于[express](https://github.com/expressjs)中的"public"文件夹或者在其他服务器比如Apache中所熟知"www"文件夹。这个中间步骤(指的是处理过程)允许Sails使用一些预编译工具比如LESS、CoffeeScript、SASS、Jade等进行处理文件。

### 静态中间件
在后台，Sails使用Express提供的[静态中间件](http://www.senchalabs.org/connect/static.html)来服务你的Assets。你可以通过[/config/http.js](http://sailsjs.org/documentation/reference/sails.config/sails.config.http.html)来配置这个中间件(比如缓存设置)。

##### `index.html`
就像大部分web服务器一样，Sails也是使用`index.html`作为网页入口。比如，你在一个新的Sails工程中创建一个文件在`assets/foo.html`，那么在浏览器中输入`http://localhost:1337/foo.html`可访问到`foo.html`文件。但是如果你创建了assets/foo/index.html，那么你将可以在`http://localhost:1337/foo/index.html`和`http://localhost:1337/foo`访问到文件。

##### 优先级
值得注意的是静态[中间件](http://stephensugden.com/middleware_guide/)的安装**是在Sails路由器之后**。所以如果你定义了一条[自定义的路由](http://www.blog.5udou.cn)，并且在你的Assets文件夹下有一个路径冲突的文件，那么那条自定义的路由将会在将这条请求到达静态中间之前拦截下来。比如，如果你创建`assets/index.html`，并且没有在你的`config/routes.js`文件中定义任何路由，那么主页将可以访问到这个文件。但是如果你定义了一条自己的路由`'/': 'FooController.bar'`，那么这条路由将会优先使用，也就是呈现给用户的不再是index.html文件而是FooController.bar。



<docmeta name="displayName" value="Assets">
