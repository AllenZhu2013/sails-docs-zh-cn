<<<<<<< HEAD
=======
# FAQ


### Can I use environment variables?

Yes! Like any Node app, your environment variables are available as `process.env`.

Sails also comes with built-in support for creating your own custom configuration settings that will be exposed directly on `sails.config`.  And whether custom or built-in, any of the configuration properties in `sails.config` can be overridden using environment variables.  See the conceptual documentation on [Configuration](http://sailsjs.com/documentation/concepts/configuration) for details.
>>>>>>> upstream/master

# FAQ
### 我能够使用环境变量吗？
可以的！就像其他的Node app，你可用的环境变量是`process.env`。

Sails也带有内建对创建你自己的那些直接暴露在`sails.config`的自定义配置设置的支持。并且无论是否配置是自定义的还是内建的，任何在`sails.config`的配置属性都可以使用环境变量重写。更多细节参考[Configuration](http://sailsjs.org/documentation/concepts/configuration)文档。

<<<<<<< HEAD
### 在哪里放置我的产品数据库认证？其他设置呢？
最简单的方法是通过修改在`config/`下的文件或者添加新的文件来为你的Sails添加配置。Sails支持立即加载环境指定的配置，所以你可以使用`config/env/production.js`。另外，具体细节参考[Configuration](http://sailsjs.org/documentation/concepts/configuration)。

对于开发环境，使用环境变量是不合适的。

对于你的其他部署或者机器相关的设置，也就是任何你想要保存为私有的认证类型，你应该使用环境变量或者你的`config/local.js`。该文件是默认包含在`.gitignore`，所以你无须担心会提交你的认证到代码库中。
=======
The easiest way to add configuration to your Sails app is by modifying the files in `config/` or adding new ones. Sails supports environment-specific configuration loading out of the box, so you can use `config/env/production.js`.  Again, see the conceptual documentation on [Configuration](http://sailsjs.com/documentation/concepts/configuration) for details.

But sometimes, you don't want to check certain configuration information in to your repository.  **The best place to put this kind of configuration is in environment variables.**

That said, for development (e.g. on your laptop) using environment variables can sometimes be kind of awkward.  So for your other deployment/machine-specific settings, namely any kind of credentials you want to keep private, you can also use your `config/local.js` file.  This file is included in your `.gitignore` file by default-- this helps prevent you from inadvertently commiting your credentials to your code repository.
>>>>>>> upstream/master

**config/local.js**

```javascript
// Local configuration
//
// Included in the .gitignore by default,
// this is where you include configuration overrides for your local system
// or for a production deployment.
//
// For example, to use port 80 on the local machine, override the `port` config
module.exports = {
    port: 80,
    environment: 'production',
    adapters: {
        mysql: {
            user: 'root',
            password: '12345'
        }
    }
}
```

### 在服务器上我应该怎样访问我的Sails app？
如果你正在使用诸如Heroku或Modulus之类的PaaS，那么很简单：只需要按照他们提供的步骤操作即可。

否则你需要获取服务器的IP地址然后`ssh`登陆上去。然后在服务器上使用命令`npm install -g sails`和`npm install -g forever`第一次地从NPM全局地安装Sails和`forever`。最后`git clone`你的工程(或者如果不是git代码库的话就`scp`代码到服务器上)到一个新的文件夹下，接着`cd`到该文件夹下，接着运行`forever start app.js`即可。

<<<<<<< HEAD
### 就性能而言Sails是如何的呢？
Sails的基本性能可以和一个标准的nodejs或者Express应用相媲美。换句话说，那就是快！在Sails和Waterline中我们已经做了一些优化，但是我们主要关注的是从我们依赖中获取的东西不是一团糟。更多细节请参考[http://serdardogruyol.com/?p=111](http://serdardogruyol.com/?p=111)。
=======
### How do I get my Sails app on the server?

If you are using a Paas like Heroku or Modulus, this is easy:  just follow their instructions.

Otherwise get the IP address of your server and `ssh` onto it.  Then `npm install -g sails` and `npm install -g forever` to install Sails and `forever` globally from NPM for the first time on the server. Finally `git clone` your project (or `scp` it onto the server if it's not in a git repo) into a new folder on the server, `cd` into it, and then run `forever start app.js`.


### What should I expect as far as performance?

Baseline performance in Sails is comparable to what you'd expect from a standard Node.js/Express application.  In other words, fast!  We've done some optimizations ourselves in Sails core, but primarily our focus is not messing up what we get for free from our dependencies.  For a quick and dirty benchmark, see [http://serdardogruyol.com/sails-vs-rails-a-quick-and-dirty-benchmark](http://serdardogruyol.com/sails-vs-rails-a-quick-and-dirty-benchmark).

The most common performance bottleneck in production Sails applications is the database.  Over the lifetime of an application with a growing user base, it becomes increasingly important to set up good indexes on your tables/collections, and to use queries which return paginated results.  Eventually as your production database grows to contain tens of millions of records, you will start to locate and optimize slow queries by hand (either by calling [`.query()`](http://sailsjs.com/documentation/reference/waterline-orm/models/query) or [`.native()`](http://sailsjs.com/documentation/reference/waterline-orm/models/native), or by using the underlying database driver from NPM).  
>>>>>>> upstream/master

在Sails的产品应用中最大的性能瓶颈是数据库。随着一个应用的运行时间加长以及用户数据变多，那么如何为你的表或者集合设置一个更好的索引并使用查询返回分页的结果。最后因为你的产品数据库发展成包含了数以千万计的记录，你就需要开始手工地定位和优化查询慢的条目(也可以通过调用[`.query()`](http://sailsjs.org/documentation/reference/waterline-orm/models/query)或[`.native()`](http://sailsjs.org/documentation/reference/waterline-orm/models/native)或使用NPM提供的底层数据库驱动)。

### 就性能而言Sails是如何的呢？
如果你在你的Sails app中使用了会话，你不应该在产品中使用内建的内存存储。内存会话存储只是为了开发而使用的工具，因为它不能扩展到多个服务器；甚至即使如果你只有一个服务器它也不能满足性能(参考[#3099](https://github.com/balderdashy/sails/issues/3099)和[#2779](https://github.com/balderdashy/sails/issues/2779))。

对于配置一个产品的存储会话指令，参考[sails.config.session](http://sailsjs.org/documentation/reference/configuration/sails-config-session)。如果你想要一起关闭会话支持，那么在你的`.sailsrc`文件中关掉`session`钩子：

<<<<<<< HEAD
=======
For instructions on configuring a production session store, see [sails.config.session](http://sailsjs.com/documentation/reference/configuration/sails-config-session).  If you want to disable session support altogether, turn off the `session` hook in your app's `.sailsrc` file:
>>>>>>> upstream/master
```javascript
"hooks": {
  "session": false
}
```


<docmeta name="displayName" value="FAQ">

