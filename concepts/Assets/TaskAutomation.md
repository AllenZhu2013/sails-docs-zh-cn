# 任务自动化
### 概述
[tasks/](http://sailsjs.org/documentation/anatomy/tasks)文件夹下包含了一系列的[grunt任务](http://gruntjs.com/creating-tasks)以及他们的[配置](http://gruntjs.com/configuring-tasks)。

这些任务主要服务于前端assets，比如样式表、脚本或者客户端的标记模板，但是它们也可以用于自动化所有重复性的零碎工作，从[browserify](https://github.com/jmreidy/grunt-browserify)的编译到[数据库的迁移](https://www.npmjs.org/package/grunt-db-migrate)。

虽然Sails为了便利绑定了一些[默认的任务](http://sailsjs.org/documentation/grunt/default-tasks)，但是因为有[成百上千的插件](http://gruntjs.com/plugins)可以选择，你完全可以用最小的effor让这些任务去自动化完成任何事情。如果你找不到你想要的插件，你也可以成为[作者](http://gruntjs.com/creating-tasks)并将[你的grunt插件发布](http://gruntjs.com/creating-plugins)到[npm](http://npmjs.org/)中。

> 如果你以前没有使用过[grunt](http://gruntjs.com/)，请查看[grunt的使用向导](http://gruntjs.com/getting-started)，该文档介绍了如何使用创建一个[gruntfile](http://gruntjs.com/sample-gruntfile)、如何安装和使用一个grunt插件。

### Assets流水线
Assets流水线指的是你组织那些注入到你的视图中的Assets的地方，相关文件是`tasks/pipeline.js`。配置这些Assets是很容易的，你可以使用[任务文件的配置](http://gruntjs.com/configuring-tasks#files)和[通配符](http://gruntjs.com/configuring-tasks#globbing-patterns)，该文件被分为3部分：

#### 注入CSS文件
这是一组将被作为`<link>`标签注入到你的html文件的css文件。这些标签将会被注入到在视图文件中任何有注释着`<!--STYLES--><!--STYLES END-->`的地方。

#### 注入JS文件
这是一组将被作为`<script>`标签注入到你的html文件的css文件。这些标签将会被注入到在视图文件中任何有注释着`<!--SCRIPTS--><!--SCRIPTS END-->`的地方。按照先后顺序依次注入到对应的位置(也就是你需要先引用那些最先被依赖的文件)

#### 注入模板文件
这是一组html文件，它们将被编译为一个jst函数并放置在一个.jst文件中。这个文件然后作为一个`<script>`标签注入到在html文件中注释着`<!--TEMPLATES--><!--TEMPLATES END-->`的地方。

> 相同的grunt通配符和任务文件配置也用在一些它们自己的任务配置js文件中

### 任务配置
配置任务就是在你的gruntfile中设置一组规则，在app启动的时候遵循这组规则进行跑任务。它们完全可以自定义，文件主要都是放在[task/config](http://sailsjs.org/documentation/anatomy/my-app/tasks/config)中。你可以修改、忽略或者替换这些grunt任务中的任意一个来满足你的需求。你也可以添加自己的Grunt任务，只需要在该目录下添加一个`someTask.js`文件来配置新的任务，然后将它注册到一个合适的父任务中去(参考`tasks/register/*.js`)。记住Sails默认自带一组有用的任务来让你的服务器跑起来并且不需要任何别的多余配置。

##### 配置一个自定义的任务
在你的项目中配置一个自定义的任务是非常简单的，使用Grunt的配置和任务API可以让你的任务变得模块化。接下去我们以创建一个新任务来替换一个已经存在的任务的例子来说明。因为我们想要使用[Handlebars](http://handlebarsjs.com/)模板引擎来替换underscore模板引擎。

+ 第一步是在你的终端中使用命令安装handlebars Grunt插件：
```bash
npm install grunt-contrib-handlebars --save-dev
```
+ 在`tasks/config/handlebars.js`创建一个配置文件，这个目录是我们放置配置文件的地方：

  ```javascript
// tasks/config/handlebars.js
// --------------------------------
// handlebar task configuration.

module.exports = function(grunt) {

  // We use the grunt.config api's set method to configure an
  // object to the defined string. In this case the task
  // 'handlebars' will be configured based on the object below.
  grunt.config.set('handlebars', {
    dev: {
      // We will define which template files to inject
      // in tasks/pipeline.js
      files: {
        '.tmp/public/templates.js': require('../pipeline').templateFilesToInject
      }
    }
  });

  // load npm module for handlebars.
  grunt.loadNpmTasks('grunt-contrib-handlebars');
};
```
+ 在asset流水线中替换路径为源文件。这个地方唯一的改动就是handelbars查找文件扩展名为.hbs的文件但是underscore模板可以在简单的html文件中。

 ```javascript
// tasks/pipeline.js
// --------------------------------
// asset pipeline

var cssFilesToInject = [
  'styles/**/*.css'
];

var jsFilesToInject = [
  'js/socket.io.js',
  'js/sails.io.js',
  'js/connection.example.js',
  'js/**/*.js'
];

// We change this glob pattern to include all files in
// the templates/ direcotry that end in the extension .hbs
var templateFilesToInject = [
  'templates/**/*.hbs'
];

module.exports = {
  cssFilesToInject: cssFilesToInject.map(function(path) {
    return '.tmp/public/' + path;
  }),
  jsFilesToInject: jsFilesToInject.map(function(path) {
    return '.tmp/public/' + path;
  }),
  templateFilesToInject: templateFilesToInject.map(function(path) {
    return 'assets/' + path;
  })
};
```

+ 包含handlebars任务到已经注册的compileAssets 和syncAssets 中。这是jst任务被使用的地方并且我们将其替换成我们新配置的handlebars 任务。

 ```javascript
// tasks/register/compileAssets.js
// --------------------------------
// compile assets registered grunt task

module.exports = function (grunt) {
  grunt.registerTask('compileAssets', [
    'clean:dev',
    'handlebars:dev',       // changed jst task to handlebars task
    'less:dev',
    'copy:dev',
    'coffee:dev'
  ]);
};

// tasks/register/syncAssets.js
// --------------------------------
// synce assets registered grunt task

module.exports = function (grunt) {
  grunt.registerTask('syncAssets', [
    'handlebars:dev',      // changed jst task to handlebars task
    'less:dev',
    'sync:dev',
    'coffee:dev'
  ]);
};
```

+ 删除jst任务配置文件。我们不再使用它所以可以直接忽略`tasks/config/jst.js`。

> 理想的情况是你将文件从你的工程和你的工程的node依赖库中删除。可以通过在命令行中运行这条命令：
```bash
npm uninstall grunt-contrib-jst --save-dev
```


### 任务触发
在[开发模式](http://sailsjs.org/documentation/reference/sails.config/sails.config.local.html?q=environment)中，Sails将会运行`默认`任务组(`tasks/register/default.js`)。这个任务组将会编译LESS、CoffeeScript和JST模板文件，然后将它们自动地从你的app的动态视图中链接到静态的HTML文件。

在产品模式中，Sails将会运行`prod`任务组([tasks/register/prod.js](http://sailsjs.org/documentation/anatomy/myApp/tasks/register/prod.js.html)).这个任务组基本和`default`任务组一样，不过除此之外还压缩你的app的脚本和样式表文件。这个可以减轻你的服务器的负载以及带宽压力。

这些任务触发器都是位于tasks/register/文件夹下的["基本"Grunt任务](http://gruntjs.com/creating-tasks#basic-tasks)。下面罗列是所有命令触发的任务参考：

##### `sails lift`

运行的是**default**任务组(`tasks/register/default.js`)

##### `sails lift --prod`

 运行的是**prod**任务组(`tasks/register/prod.js`)

##### `sails www`

 运行的是**build**任务组(`tasks/register/build.js`)，编译所有的Assets到`www`子文件夹而不是`./tmp/public`,使用的是在参考中的相对路径。这个允许将一些静态资源服务于Apache或Ngnix而不用一直依赖于['www middleware'](http://sailsjs.org/documentation/concepts/Middleware)。

##### `sails www --prod(production)

运行的是**buildProd**任务组(`tasks/register/buildProd.js`)，除了做的事情和build任务组一样之外还可以优化Assets。

你也许通过指定设置NODE_ENV运行其他任务并且在tasks/register/中创建一个相同名字的任务列表。比如，如果NODE_ENV是QA，sails将会运行tasks/register/QA.js如果该文件存在的话。


<docmeta name="displayName" value="Task Automation">
