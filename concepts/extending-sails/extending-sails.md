# Extending Sails
为了和Node的本质保持一致，Sails尽可能地减小核心代码的大小，转移所有的代码除了那些成独立模块_*的最重要的函数。下面有三种类型的扩展你可以添加到Sails中：

+ [**构造器**](http://sailsjs.org/documentation/concepts/extending-sails/Generators)：为了在Sails CLI中功能性地添加和重写。比如：sails-generate-model允许你用命令行`sails generate model foo`创建模型；
+ [**适配器**](http://sailsjs.org/documentation/concepts/extending-sails/Adapters)：为了集成新的数据源包括数据库、API乃至硬件到WaterLine(Sails的ORM)。*比如*：[sails-postgresql](https://www.npmjs.com/package/sails-postgresql)，为Sails做的官方[PostgreSQL](http://www.postgresql.org/)适配器。
+ [**钩子**](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)：为了在Sails运行的时候改写或者注入新的功能。*比如*：[sails-hook-autoreload](https://www.npmjs.com/package/sails-hook-autoreload)，可以为一个Sails的工程的API添加自动实时刷新而不需要手动重启服务器。

如果你对为Sails开发插件感兴趣，你最有可能做的就是[钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)。

<a name="foot1">*</a> <sub>_Core hooks，比如`http`，`request`等，都是绑定在Sails中立即可用的钩子。它们可以在你的`.sailsrc`文件或者当可编程式地启动Sails的时候指定一个`hooks`配置来禁用掉。

<docmeta name="displayName" value="Extending Sails">
