# Configuration
### 概述
当Sails尽职尽责地遵守[惯例优于配置](http://en.wikipedia.org/wiki/Convention_over_configuration)(COC)的原则的时候，对于了解如何自定义这些便利的默认值是非常重要的。在Sails中绝大部分的每一次会话，都会伴随着一组配置选项以允许你调整或者重写某些配置以满足你的需求。本章就是主要来描述那些在Sails可用的配置选项。

Sails app是[可编程配置的](https://github.com/mikermcneil/sails-generate-new-but-like-express/blob/master/templates/app.js#L15)，通过指定[环境变量](http://en.wikipedia.org/wiki/Environment_variable)或者命令行参数，通过改变本地或者全局的[.sailsrc](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html)文件，或者(更常见的)使用在新工程中的`config/`文件夹下的样板配置文件。当你的app运行的时候这些配置可以通过`sails`这个全局变量中的`sails.config`访问到。

### 标准配置文件(`config/*`)
在Sails的app中默认包含一些配置文件，这些样板文件包含一些内嵌的注释，这些注释是为了给你提供一个快速流畅地参考而不需要在你的编辑器和参考文档中来回切换。

在大部分的情况下，`sails.config`实体中顶层关键词(比如`sails.config.views`)都对应一个特殊的配置文件(比如`config/views.js`)；然而在你的`config/`目录下的文件可以按照你喜欢的方式排列。因为最重要的是设置的名字(也就是关键词)而不是这个文件来自哪里。

比如，假设你添加了一个新的文件，`config/foo.js`：

```js
// config/foo.js
// The object below will be merged into `sails.config.blueprints`:
module.exports.blueprints = {
  shortcuts: false
};
```

为了个别特殊的配置选项有一个详尽的参考，以及它们默认的文件位置，在这个章节中都有一个详尽的参考页，或者你可以参考[Sails APP的文件架构](http://sailsjs.org/documentation/anatomy)的[config/](http://sailsjs.org/documentation/anatomy/myApp/config)小节以有一个大概的预览。

### 环境专用文件(`config/env/*`)
在标准配置文件中一些专用的设置一般都会在所有的环境(比如开发环境、产品环境、测试环境等)中可用。如果你只想要某些设置在某些特定的环境下生效，你可以使用这个特殊的环境专用文件和文件夹：

+ 任何存放在`/config/env/<environment-name>`文件夹下的文件将被加载当Sails运行在对应的`<environment-name>`的环境。比如，只有当Sails运行在production的环境的时候才会去加载`config/env/production`的文件。
+ 任何保存的文件路径为`config/env/<environment-name>.js`的文件都会被加载当Sails运行在`<environment-name>`的环境下，并且会被合并到在环境专用子文件夹下的设置的顶部。比如，在`config/env/production.js`中的设置优先级高于那些在`config/env/production`的文件的设置。

默认，你的app运行在“开发”模式下。改变你的app运行环境的建议方法是通过使用`NODE_ENV`环境变量：

```
NODE_ENV=production node app.js
```
> `production`环境是很特殊的，它基于你的配置，并使能压缩、缓存、精简化等。
>
>  同时也请注意如果你在使用`config/local.js`的话，该文件导出的配置优先级高于环境变量指定的配置文件

### `Config/local.js`文件
你可以使用`config/local.js`文件来为你的本地环境(比如你的电脑)做配置。在这个文件中的设置的优先级将会高于其他配置文件除了[.sailsrc](http://sailsjs.org/documentation/concepts/Configuration/usingsailsrcfiles.html)。因为它的本身是为了本地使用，所以这个文件不应该归入版本管理库中(这也是将此文件加入到`.gitignore`的原因)。你可以使用`local.js`来存储本地数据库的设置，改变你的电脑中服务器运行的端口号等等。

更多细节参考：http://sailsjs.org/documentation/concepts/Configuration/localjsfile.html

### 在服务器上访问`Sails.config`
在Sails app实例中`config`对象是可用的。默认这个对象是全局变量，因此可以在你的服务器中的任何地方使用这个变量。

##### 例子

```javascript
// This example checks that, if we are in production mode, csrf is enabled.
// It throws an error and crashes the app otherwise.
if (sails.config.environment === 'production' && !sails.config.csrf) {
  throw new Error('STOP IMMEDIATELY ! CSRF should always be enabled in a production deployment!');
}
```

### 使用环境变量直接设置`sails.config`值
除了使用配置文件，你也可以在运行Sails的时候通过添加配置关键词前缀`sails_`来在命令行上设置个别的配置值，并使用双下划线(`__`)来分离嵌入的关键名称。比如，你可以运行下面的命令来设置[CORS origin](http://sailsjs.org/documentation/concepts/security/cors)(`sails.config.cors.origin`)到"http://somedomain.com"。

```javascript
sails_cors__origin="http://somedomain.com" sails lift
```

这个值*只会*在这个特定的Sails实例的生命周期内有效，并且会重写覆盖掉在这个配置文件下的任何值。

> 对于上面的规则有一些例外的情况: `NODE_ENV` and `PORT`.
> + `NODE_ENV` 对于任何的Node.js App都是一种约定俗成的变量.  当将它设置为`'production'`, 将会生效在变量[`sails.config.environment`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigenvironment)中.
> + 类似地, `PORT`变量也是等同于 [`sails.config.port`](http://sailsjs.org/documentation/reference/configuration/sails-config#?sailsconfigport).  这个是严格规定的并且是向后兼容的。
>
> 这里有一个类似的例子，可以同时设置这些环境变量：
>
> ```bash
> PORT=443 NODE_ENV=production sails lift
> ```


### 自定义配置


Sails识别许多不同的配置，以及在不同顶层关键词的域名空间(比如`sails.config.sockets`和`sails.config.blueprints`)。然而你也可以使用`sails.config`来自定义自己的配置(比如`sails.config.someProprietaryAPI.secret`)。

##### 例子：

 ```javascript
// config/linkedin.js
module.exports.linkedin = {
  apiKey: '...',
  apiSecret: '...'
};
```

```javascript
// In your controller/service/model/hook/whatever:
// ...
var apiKey = sails.config.linkedin.apiKey;
var apiSecret = sails.config.linkedin.apiSecret;
// ...
```


### 在CLI中配置`Sails`
当说起配置，绝不部分时间你都会关注在为一个特殊的app管理实时配置：端口、数据库连接等等。然而这对于自定义Sails CLI本身是有帮助。为了简化你的工作流，减少重复的任务，实现客户自动构建等等，Sails v0.10添加了一个强有力的工具来实现这些目标。

[`.Sailsrc`文件](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html)文件在Sails app中所有的配置源文件中是唯一的。它也可以用来配置Sails的CLI，既可以配置全系统范围的也可以某个文件夹内的。虽然实现这个最主要的原因是为了自定义当运行`sail new`和`sails generate`的时候使用的生成器，但是它也可以用来安装你自定义的生成器或者应用硬编码配置重载。

因为Sails会在当前工作的目录的原始文件夹下查找"最近的"`.sailsrc`，所以你可以安全地使用这个文件去配置那些你不能上传到云端代码库的敏感配置(比如**数据库密码**)。只需要包含一个`.sailsrc`文件到你的"$HOME"目录下即可。更多细节参考[.sailsrc的文档](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html)。

### 注意

> 在sails.config配置中的内嵌含义是，在一些情况下，当服务器运行起来的时候会被拦截。换句话说，在实时运行的时候改变某些配置将不会生效。比如在你的app运行的时候为了改变端口号，你不能仅仅简单修改sails.config.port。你需要在一个配置文件中修改或者重写设置或者通过命令行参数等等做法，然后重启服务器，这样才能生效。



<docmeta name="displayName" value="Configuration">
