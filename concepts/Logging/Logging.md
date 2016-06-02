# Logging

Sails自带一个简单的内建的日志器叫做[`captains-log`](https://github.com/balderdashy/captains-log)。它的使用非常类似于Node中的[`console.log`](https://nodejs.org/api/console.html#console_console_log_data)，但是额外添加了一些特性。换句话说就是支持多种带着彩色和前缀的终端输出的日志等级记录。日志器服务于两个目的：

+ 它可以从Sails内部框架中发射出警告、错误和其他控制台输出；
+ 它可以在你的应用代码中发射[自定义的事件或消息](http://sailsjs.org/documentation/concepts/logging/custom-log-messages)

### 配置
Sails的日志器的配置是[`sails.config.log`](http://sailsjs.org/documentation/reference/configuration/sails-config-log)或者更便捷的是修改文件([`config/log.js`](http://sailsjs.org/documentation/anatomy/my-app/config/log-js))。

### 日志等级
当使用内建的日志器时，Sails将会输出那些在该*等级之上(包含该等级)*的日志消息到终端。这个日志级别是标准化的并且用于那些从socket.io，Waterline和其他依赖的组件生成的输出。日志级别的层级关系以及它们的优先级关系如下表所示：

| Priority | Level     | Log fns that produce visible output   |
|----------|-----------|:--------------------------------------|
| 0        | silent    | _N/A_
| 1        | error     | `.error()`            |
| 2        | warn      | `.warn()`, `.error()` |
| 3        | debug     | `.debug()`, `.warn()`, `.error()` |
| 4        | info      | `.info()`, `.debug()`, `.warn()`, `.error()` |
| 5        | verbose   | `.verbose()`, `.info()`, `.debug()`, `.warn()`, `.error()` |
| 6        | silly     | `.silly()`, `.verbose()`, `.info()`, `.debug()`, `.warn()`, `.error()` |

#### 注意：
+ [默认的日志等级](http://sailsjs.org/documentation/reference/configuration/sails-config-log)是**info**。当你的app的等级设置为“info”的时候，Sails将记录有限的有关服务器状态的信息；
+ 当你的app运行自动化测试的时候，一般将其日志等级设置为**error**；
+ 当你的日志等级设置为**verbose**，Sails将会记录Grunt的输出，还有更多关于路由、模型、钩子等等的加载信息。
+ 当你的等级设置为**silly**，Sails将会输出路由绑定的内部信息和其他详细的框架生命周期信息、诊断信息以及实现细节；



<docmeta name="displayName" value="Logging">
