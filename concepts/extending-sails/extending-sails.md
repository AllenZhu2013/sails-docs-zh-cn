# Extending Sails
为了和Node的本质保持一致，Sails尽可能地减小核心代码的大小，转移所有的代码除了那些成独立模块_*的最重要的函数。下面有三种类型的扩展你可以添加到Sails中：

+ [**构造器**](http://sailsjs.org/documentation/concepts/extending-sails/Generators)：为了在Sails CLI中功能性地添加和重写。比如：sails-generate-model允许你用命令行`sails generate model foo`创建模型；
+ [**适配器**](http://sailsjs.org/documentation/concepts/extending-sails/Adapters)：为了集成新的数据源包括数据库、API乃至硬件到WaterLine(Sails的ORM)。*比如*：[sails-postgresql](https://www.npmjs.com/package/sails-postgresql)，为Sails做的官方[PostgreSQL](http://www.postgresql.org/)适配器。
+ [**钩子**](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)：为了在Sails运行的时候改写或者注入新的功能。*比如*：[sails-hook-autoreload](https://www.npmjs.com/package/sails-hook-autoreload)，可以为一个Sails的工程的API添加自动实时刷新而不需要手动重启服务器。

<<<<<<< HEAD
如果你对为Sails开发插件感兴趣，你最有可能做的就是[钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)。
=======
+ [**Generators**](http://sailsjs.com/documentation/concepts/extending-sails/Generators) - for adding and overriding functionality in the Sails CLI.  *Example*: [sails-generate-model](https://www.npmjs.com/package/sails-generate-model), which allows you to create models on the command line with `sails generate model foo`.
+ [**Adapters**](http://sailsjs.com/documentation/concepts/extending-sails/Adapters) - for integrating Waterline (Sails' ORM) with new data sources, including databases, APIs, or even hardware. *Example*: [sails-postgresql](https://www.npmjs.com/package/sails-postgresql), the official [PostgreSQL](http://www.postgresql.org/) adapter for Sails.
+ [**Hooks**](http://sailsjs.com/documentation/concepts/extending-sails/Hooks) - for overriding or injecting new functionality in the Sails runtime.  *Example*: [sails-hook-autoreload](https://www.npmjs.com/package/sails-hook-autoreload), which adds auto-refreshing for a Sails project's API without having to manually restart the server.

If you&rsquo;re interested in developing a plugin for Sails, you will most often want to make a [hook](http://sailsjs.com/documentation/concepts/extending-sails/Hooks).  

<a name="foot1">*</a> <sub>_Core hooks_, like `http`, `request`, etc. are hooks which are bundled with Sails out of the box.  They can be disabled by specifying a `hooks` configuration in your `.sailsrc` file, or when lifting Sails programatically.</sub>
>>>>>>> upstream/master

<a name="foot1">*</a> <sub>_Core hooks，比如`http`，`request`等，都是绑定在Sails中立即可用的钩子。它们可以在你的`.sailsrc`文件或者当可编程式地启动Sails的时候指定一个`hooks`配置来禁用掉。

<docmeta name="displayName" value="Extending Sails">
