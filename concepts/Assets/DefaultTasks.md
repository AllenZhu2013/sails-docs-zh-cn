# 默认任务

### 概述
绑定到Sails的Assets流水线是一组配置为常见的默认值的Grunt任务，这样设计主要是让你的工程变得更加一致和富有成效。整个前端Assets工作流完全可以客制化，因为它创造性地提供了一些默认的任务。Sails让你更加容易地[配置新的任务](http://sailsjs.org/documentation/concepts/Assets/TaskAutomation.html?q=task-configuration)以符合自己的需求。<!-- change link to: /documentation/concepts/assets/task-automation#?task-configuration once new site is live -->

下面罗列的是Sails默认的Grunt配置以帮助你更有成效搭建服务器:
+ 自动LESS编译
+ 自动JST编译
+ 自动coffeeScript编译
+ 可选的自动Assets注入、压缩、和级联
+ 创建一个服务器public目录
+ 文件的监控以及同步
+ 在产品模式下Assets的优化

### 默认的Grunt任务行为
下面是包含在你的Sails工程中的Grunt任务的简单描述，包含任务的作用以及任务的链接文档。

##### Clean
这个任务用于清空`./tmp/public`文件夹下面的内容。

[使用说明](https://github.com/gruntjs/grunt-contrib-clean)

##### coffee
编译在`assets/js`目录下的coffeeScript文件然后将它们放在`./tmp/public`。

[使用说明](https://github.com/gruntjs/grunt-contrib-coffee)

##### concat
级联JS文件和CSS文件并放在`./tmp/public/concat`目录下。

[使用说明](https://github.com/gruntjs/grunt-contrib-concat)

##### copy
**dev task config**拷贝sails的Assets目录下所有的目录和文件除了coffeescript和less文件到`./tmp/public`目录；
**build task config**拷贝`./tmp/public`目录下所有的目录和文件到www目录。

[使用说明](https://github.com/gruntjs/grunt-contrib-copy)

##### cssmin
压缩css文件并放到`./tmp/public`目录下

[使用说明](https://github.com/gruntjs/grunt-contrib-cssmin)

##### jst
预编译underscore模板成一个`.jst`文件(也就是利用HTML模板文件并将它们变为微小的JavaScript函数)，这个可以加速客户端模板的渲染并减少带宽压力。

[使用说明](https://github.com/gruntjs/grunt-contrib-jst)

##### less
编译less文件为css文件。只有`assets/styles/importer.less`被编译。这个允许你在其他样式表之前控制你自己的顺序，也就是导入你的依赖关系、mixins、变量、重置等。

[使用说明](https://github.com/gruntjs/grunt-contrib-less)

##### sails-linker
自动为JS文件和CSS文件注入`<script>`标签和`<link>`标签。同时也使用`<script>`标签自动链接一个外部包含预编译模板的文件。关于该任务更加详细的说明请参考[这里](https://github.com/balderdashy/sails-generate-frontend/blob/master/docs/overview.md#a-litte-bit-more-about-sails-linking)，但是最大的收获是脚本和样式表的注入**只有**发生在那些包含`<!--SCRIPTS--><!--SCRIPTS END-->`且/或`<!--STYLES--><!--STYLES END-->`标签的文件。这些标签默认包含在一个新的Sails工程中的**views/layout.ejs**文件，如果你不想使用这个链接器，你可以直接删除这些标签。

[使用说明](https://github.com/Zolmeister/grunt-sails-linker)

##### sync
该任务保持目标目录与源目录同步。它很类似于grunt-contrib-copy任务但是它只拷贝那些真正改变的文件。主要同步的文件夹是`assets/`和`.tmp/public/`，直接重新覆盖掉那些改动的文件

[使用说明](https://github.com/tomusdrw/grunt-sync)


##### uglify
压缩客户端的JS文件


[使用说明](https://github.com/gruntjs/grunt-contrib-uglify)

##### 、watch
当监控的文件类型被添加、修改或者删除的时候运行预定义好的那些任务。监控的是`assets/`文件夹下的文件改变，然后运行恰当的任务(比如less和jst编译)。这个允许你可以实时看到你的Assets目录下文件的改变而不需要重启服务器。

[使用说明](https://github.com/gruntjs/grunt-contrib-watch)


<docmeta name="displayName" value="Default Tasks">
