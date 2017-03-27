<<<<<<< HEAD
# 禁用Grunt
在Sails中如果想要禁用Grunt，简单地删掉Gruntfile(且/或 [`tasks/`](http://sailsjs.org/documentation/anatomy/myApp/tasks)文件夹)就行了。你也可以禁用Grunt的钩子。只需要在`.sailsrc`文件中将`grunt`的属性置为`false`即可，如下：
=======
# Disabling Grunt

To disable Grunt integration in Sails, simply delete your Gruntfile (and/or [`tasks/`](http://sailsjs.com/documentation/anatomy/tasks) folder). You can also disable the Grunt hook. Just set the `grunt` property to `false` in `.sailsrc` hooks like this:
>>>>>>> upstream/master

```json
{
    "hooks": {
        "grunt": false
    }
}
```

### 我可以为SASS、Angular、客户端Jade模板自定义任务吗？
当然可以！只需要在/task文件夹下替换掉相关的grunt任务，或者添加一个新的任务，比如[SASS](https://github.com/sails101/using-sass)。

<<<<<<< HEAD
如果你仍然想使用grunt作为他用，但是不想使用任何的web前端材料，只需要删掉你的工程的assets文件夹并从`grunt/register`和`grunt/config`文件夹下删除前端定向的任务即可。你也可以运行命令行：`sails new myCoolApi --no-frontend`来忽略asset文件夹和前端定向的grunt任务。还可以替换你的`sails-generate-frontend`模块为其他备选的社区生成器，或者[创建你自己的生成器](https://github.com/balderdashy/sails-generate-generator)。这样允许`sails new`为原始的iOS apps，Android apps，Cordova apps，SteroidsJS apps等创建样板文件。
=======
If you still want to use Grunt for other purposes, but don't want any of the default web front-end stuff, just delete your project's assets folder and remove the front-end oriented tasks from the `tasks/register/` and `tasks/config/` folders.  You can also run `sails new myCoolApi --no-frontend` to omit the assets folder and front-end-oriented Grunt tasks for future projects.  You can also replace your `sails-generate-frontend` module with alternative community generators, or [create your own](https://github.com/balderdashy/sails-generate-generator).  This allows `sails new` to create the boilerplate for native iOS apps, Android apps, Cordova apps, SteroidsJS apps, etc.
>>>>>>> upstream/master



<docmeta name="displayName" value="Disabling Grunt">

### 注意
当按照上面删除grunt的钩子之后你必须指定如下的配置在`.sailsrc`文件，这是为了让你的asset能够正常服务于服务器否则所有的asset都将返回`404`。

```json
{
    "paths": {
        "public": "assets"
    }
}
```
