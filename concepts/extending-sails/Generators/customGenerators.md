# 自定义生成器

### 概述
要说Sails中哪个可以自动化重复工作来让你编程更加容易的话那非**Generator**莫属。*Generators*可以在你的Sails工程中使用命令行通过模板自动生成一些文件。实际上，Sails内核使用*generators*来创建Sails工程。所以当你输入：

 ```sh
~/ $ sails new myProject
```

...sails便会使用*generators*来构建一个有初始目录结构的Sails app，结构如下：

 ```javascript
myProject
       |_api
       |_assets
       |_config
       |_node_modules
       |_tasks
       |_views
.gitignore
.sailsrc
app.js
Gruntfile.js
package.json
README.md

```


在Sails内核中*generators*的其他例子：

+ sails-generate-adapter
+ sails-generate-backend
+ sails-generate-controller
+ sails-generate-frontend
+ sails-generate-model
+ sails-generate-new
+ sails-generate-views
+ sails-generate-views-jade
+ 虽然不是一个独立的模块，但是可以通过`sails generate api`访问其他的*generators*

为了开始生成一个生成器的过程你可以使用`sails-generate-generator`。

> **注意：**通过调用generators来创建*一个generators*的想法看起来像是疯狂的无限循环但是请不要怀疑这种做法，因为它不会创建一个虫洞到另外一个邪恶的世界的。

### 创建一个生成器
首先我们需要一个Sails工程。如果你还没有创建那么可以在你的终端输入：

 ```sh
~/ $ sails new myProject
```

`cd`进`myProject`或者任何已经存在的Sails工程然后在命令行中创建一个名为**awesome**的*generator*：

 ```sh
~/ $ sails generate generator awesome
```

如果你看到提示：`info: Created a new generator!`，那说明generator已经创建成功了。

### 使能一个生成器
为了使能*generator*你需要通过`\myProject\.sailsrc`来告诉Sails。如果你正在使用一个已存在的生成器那么你将会在`.sailsrc`中链接到一个npm模块然后使用`npm install`安装它。因为你正在开发一个生成器，所以你可以直接地链接。为了创建链接请回到终端然后`cd`进入`awesome` *generator*文件夹并输入：

```sh
~/ $  pwd
```

`pwd`命令将会返回一个完整的*generator*的文件夹路径(比如：`/Users/irl/sails_projects/myProject/awesome`)。

拷贝该路径然后打开`myProject/.sailsrc`。在`modules`属性中添加`awesome`关键词并粘贴刚才复制的`awesome` *generator*路径。

> **注意：**你可以命名*generator*为任意你想要的名字，现在我们只是假设为`awesome`：

```javascript
{
  "generators": {
    "modules": {
      "awesome": "/Users/irl/sails_projects/myProject/awesome"
    }
  }
}
```

> **注意：**你在`.sailsrc`文件中命名的generator的名字将会用在终端命令行中执行。

最后，你需要在终端中做一个`npm install`，这是为了安装那些添加到生成器的`package.json`需要的模块。

### 使用生成器
回到终端并输入：`sails generate awesome example`。让我们看看这会生成些什么。

#### Generator会创建些什么？
使用文本编辑器打开你的工程，你会注意到有一个新创建的文件夹叫做`hey_look_a_folder`，并且还有一个名为`example`的文件：

```javascript
/**
 * This is just an example file created at Wed Jun 04 2014 17:35:59 GMT-0500 (CDT).
 *
 * You can use underscore templates, see?
 */

module.exports = function () {
  // ...
};
```

这个文件夹和文件表明了*generator*的强大，因为它不仅创建了元素而且还使用命令行中的`arguments`来影响文件的内容。比如，文件名称--`example`，使用了命令行参数`sails generate awesome example`的一个元素。

### 基本的生成器配置
所有有关`awesome` *generator*的配置都包含在`\myProjects\awesome\Generator.js`。`Generator.js`的主要部分是`before()`函数和`targets`目录。

> **注意：**我们指的是使用`{}`作为一个目录的JavaScript对象。

### 配置before()函数
让我们仔细查看`myProject/awesome/Generator.js`文件：

 ```javascript
...
before: function (scope, cb) {

    // scope.args are the raw command line arguments.
    if (!scope.args[0]) {
      return cb( new Error('Please provide a name for this awesome.') );
    }

    // scope.rootPath is the base path for this generator
    if (!scope.rootPath) {
      return cb( INVALID_SCOPE_VARIABLE('rootPath') );
    }

    // Attach defaults
    _.defaults(scope, {
      createdAt: new Date()
    });

    // Decide the output filename for use in targets below:
    scope.filename = scope.args[0];

    // Add other stuff to the scope for use in our templates:
    scope.whatIsThis = 'an example file created at '+scope.createdAt;

    // When finished, we trigger a callback with no error
    // to begin generating files/folders as specified by
    // the `targets` below.
    cb();
  },
  ...
  ```


每个_generator_都可以访问到`scope`目录，该目录在_generator_被执行的时候你想要获取参数的时候有用。

如果你的默认`awesome` _generator_是一个新的关键词，`createdAt:`在scope中被创建。我们临时看一下某个模板的这个字段：

```javascript
...
// Attach defaults
    _.defaults(scope, {
      createdAt: new Date()
    });
...
```

接下去，当执行`awesome` *generator*(比如`sails generate awesome <theargument>`)的时候，`scope.arg`中参数数组是可用的。在我们默认的`awesome` *generator*，属性`filename`被添加到`scope`中并为`scope.arg`数组中的第一个元素的值赋值。比如：

```javascript
...
scope.filename = scope.args[0];
...
```

最后，另外一个属性(比如scope.whatIsThis)也被添加到`scope`目录。

```javascript
...
scope.whatIsThis = 'an example file created at '+scope.createdAt;
...
```

#### 配置目标目录
现在我们仔细查看`myProject\awesome\Generator.js`中的`targets`字段来更好理解这个文件夹(比如hey_look_a_folder)和文件(比如example)是怎样生成的。

```javascript
...
targets: {

    // Usage:
    // './path/to/destination.foo': { someHelper: opts }

    // Creates a dynamically-named file relative to `scope.rootPath`
    // (defined by the `filename` scope variable).
    //
    // The `template` helper reads the specified template, making the
    // entire scope available to it (uses underscore/JST/ejs syntax).
    // Then the file is copied into the specified destination (on the left).
    './:filename': { template: 'example.template.js' },

    // Creates a folder at a static path
    './hey_look_a_folder': { folder: {} }

  },
...
```

·template`和`folder`辅助者看起来很像路由。这些辅助者执行的操作就像它们的名字一样。

#####  _template_ helper
对*template*辅助者基于一个模板创建一些文件一点儿也不会感到惊讶。记住，模板中是可以访问到`scope`字段的。

```javascript
...
'./:filename': { template: 'example.template.js' },
...
```
左手边的参数指定了路径和文件名称，而右边参数则是表明*generator*会使用哪个模板去创建文件。注意你在`before`函数中的`scopre.args`的第一个参数中的`scope.filename`中使用`filename`。这个模板可以在`myProject\awesome\templates`可以找到。在`awesome` *generator*你正在使用`example.template.js`：

```javascript
/**
 * This is just <%= whatIsThis %>.
 *
 * You can use underscore templates, see?
 */

module.exports = function () {
  // ...
};
```

> **注意**：在属性scope中的`whatIsThis`是作为你重新调用使用的createdAt：属性在`before`函数中创建。

#####  _folder_ helper
_folder_辅助者用来生成文件夹。

```javascript
...
'./hey_look_a_folder': { folder: {} }
...
```

左手边的参数指定了路径和文件名称，而右边参数指定任何可选地参数。比如，默认，如果在那个位置中已经存在一个文件夹，那么将会出现一个错误：`Something else already exists at ::<path of folder>`。如果你想要*generator*去重写覆盖一个已经存在的文件夹你有两种方法：你可以在选项参数中通过指定`force:true`来让*folder*辅助者覆盖一个已经存在的文件夹：

```javascript
...
'./hey_look_a_folder': { folder: { force: true} }
...
```
你也可以在命令行中执行*generator*的时候使用`--force`参数来配置所有的辅助者覆盖已经存在的文件夹：

```sh
~/ $ sails generate awesome test --force
```

### 在一个生成器中使用一个生成器
为了利用其它编程者的劳动成果，*generators*可以被其它的*generators*使用。这就是传递给所有的*generators*的`scope`字段的强大之处。

比如，Sails内核有一个叫做`sails-generate-model`的生成器。因为它是内建到Sails内核的，所以无需安装。将它添加到我们的awesome生成器是很容易的。在`myProject\awesome\Generator.js`通过插入`./': ['model']`来包含它：

```javascript
...
targets: {

    // './:filename': { template: 'example.template.js' },

    './': ['model'],

    // './hey_look_a_folder': { folder: {} }

  },
...
```
> **注意**：通过使用`./`作为路径，生成器被执行的任何文件夹中的的任何模型都会被放在在`\api\models`文件夹中。


就是这么简单！现在我们在awesome生成器中创建一个模型。在终端中输入：

```sh
~/ $ sails generate awesome user name:string email:email
```

如果你查看了`myProject\api\models`那么你将会看到一个新的文件叫做`User.js`已经被创建了并且包含了之前指定的模型属性：

 ```javascript
/**
* User
*
* @description :: TODO: You might write a short summary of how this model works and what it represents here.
* @docs        :: http://sailsjs.com/#!documentation/models
*/

module.exports = {

  attributes: {

    name : { type: 'string' },

    email : { type: 'email' }
  }
};
```
### 福利：发布你的生成器到npmjs.org
为了发布awesome生成器到npmjs.org，请进入`myProject\awesome\package.json`文件然后改变它的文件名、作者和其他基本的信息(比如许可证)。

从`myProject\awesome`文件夹中在终端输入：

```sh
~/ $ npm publish
```
<<<<<<< HEAD
=======
>**Note:**  If you don't already have an NPM account, go to [npmjs.org](https://www.npmjs.org/) and create one.
>>>>>>> upstream/master

> **注意**：如果你还没有一个NPM账号，可以到(npmjs.org)[https://www.npmjs.org/]中创建一个

想要取消发布某个模块可以输入：

```sh
~/ $  npm unpublish --force
```

改变`myProject\.sailsrc`为：

```javascript
{
  "generators": {
    "modules": {
      "awesome": "whatever you named the module in package.json"
    }
  }
}
```


从awesome *generator*文件中在终端输入：

```sh
~/ $ npm install
```



<docmeta name="displayName" value="Custom Generators">
