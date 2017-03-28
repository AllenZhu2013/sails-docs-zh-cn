
# FAQ
### 我能够使用环境变量吗？
可以的！就像其他的Node app，你可用的环境变量是`process.env`。

Sails也带有内建对创建你自己的那些直接暴露在`sails.config`的自定义配置设置的支持。并且无论是否配置是自定义的还是内建的，任何在`sails.config`的配置属性都可以使用环境变量重写。更多细节参考[Configuration](http://sailsjs.org/documentation/concepts/configuration)文档。

### 在哪里放置我的产品数据库认证？其他设置呢？
最简单的方法是通过修改在`config/`下的文件或者添加新的文件来为你的Sails添加配置。Sails支持立即加载环境指定的配置，所以你可以使用`config/env/production.js`。另外，具体细节参考[Configuration](http://sailsjs.org/documentation/concepts/configuration)。

对于开发环境，使用环境变量是不合适的。

对于你的其他部署或者机器相关的设置，也就是任何你想要保存为私有的认证类型，你应该使用环境变量或者你的`config/local.js`。该文件是默认包含在`.gitignore`，所以你无须担心会提交你的认证到代码库中。

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

### 就性能而言Sails是如何的呢？
Sails的基本性能可以和一个标准的nodejs或者Express应用相媲美。换句话说，那就是快！在Sails和Waterline中我们已经做了一些优化，但是我们主要关注的是从我们依赖中获取的东西不是一团糟。更多细节请参考[http://serdardogruyol.com/?p=111](http://serdardogruyol.com/?p=111)。

在Sails的产品应用中最大的性能瓶颈是数据库。随着一个应用的运行时间加长以及用户数据变多，那么如何为你的表或者集合设置一个更好的索引并使用查询返回分页的结果。最后因为你的产品数据库发展成包含了数以千万计的记录，你就需要开始手工地定位和优化查询慢的条目(也可以通过调用[`.query()`](http://sailsjs.org/documentation/reference/waterline-orm/models/query)或[`.native()`](http://sailsjs.org/documentation/reference/waterline-orm/models/native)或使用NPM提供的底层数据库驱动)。

### 就性能而言Sails是如何的呢？
如果你在你的Sails app中使用了会话，你不应该在产品中使用内建的内存存储。内存会话存储只是为了开发而使用的工具，因为它不能扩展到多个服务器；甚至即使如果你只有一个服务器它也不能满足性能(参考[#3099](https://github.com/balderdashy/sails/issues/3099)和[#2779](https://github.com/balderdashy/sails/issues/2779))。

对于配置一个产品的存储会话指令，参考[sails.config.session](http://sailsjs.org/documentation/reference/configuration/sails-config-session)。如果你想要一起关闭会话支持，那么在你的`.sailsrc`文件中关掉`session`钩子：

```javascript
"hooks": {
  "session": false
}
```


<docmeta name="displayName" value="FAQ">

