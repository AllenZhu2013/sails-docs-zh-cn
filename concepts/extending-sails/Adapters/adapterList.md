# 罗列可用的适配器
这个文件已经更新到最新的了，综合了在Sails,js框架中所有可用的适配器列表。如果我们遗漏了某个或者你想添加你自己编写的适配器，只需要提交一个Pull Request到这个文件，然后添加到这个列表。

### 官方支持的适配器
##### sails-disk
地址：https://github.com/balderdashy/sails-disk/

该适配器用于写数据到你的电脑硬盘或者一个挂载的网络硬盘。在规模性地部署产品的时候不适合使用这个适配器，但是对于一些非常小的工程以及一些不需要设置一个数据库的开发模式的时候就比较适合了。这个适配器绑定在Sails中并且无需配置立即可用。

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming

##### sails-memory
地址：https://github.com/balderdashy/sails-memory/

很类似于Disk适配器，但是它不会往硬盘写数据，所以它是易失性的。在规模性地部署产品的时候不适合使用这个适配器，但是对于很少的或者没有硬盘空间的系统做开发的时候就特别有用。

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming

##### sails-mysql
地址：https://github.com/balderdashy/sails-mysql/

MySQL是世界上最流行的关系型数据库。http://en.wikipedia.org/wiki/MySQL

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming
+ Migratable

##### sails-postgres
地址：https://github.com/balderdashy/sails-postgresql/

[PostgreSQL](http://en.wikipedia.org/wiki/PostgreSQL)是另外一个流行的关系型数据库。

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming
+ Migratable


##### sails-mongo
地址：https://github.com/balderdashy/sails-mongo/

[MongoDB](http://en.wikipedia.org/wiki/MongoDB)是非常流行的NoSQL数据库。

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming


##### sails-redis
地址：https://github.com/balderdashy/sails-redis/

[Redis](http://redis.io/)是一个开源的，采用BSD许可证，高级的键值存储适配器。

##### 接口实现：
+ Semantic
+ Queryable

### 社区支持的适配器
##### sails-orientdb
地址：https://github.com/appscot/sails-orientdb

[OrientDB](http://en.wikipedia.org/wiki/OrientDB)是一个开源的同时支持文档和图形DBMSs特性的NoSQLDBMS。

##### 接口实现：
+ Semantic
+ Queryable
+ Streaming
+ Migratable

##### sails-filemaker
地址：https://github.com/geistinteractive/sails-filemaker


[FileMaker](https://en.wikipedia.org/wiki/FileMaker)是一个跨平台的关系型数据库和开发平台。自从1998年之后该功能归属于与Apple并由Apple发布。

##### 接口实现：
+ Semantic


你有自己写的Sails适配器吗？赶紧提交一个PR到这个文件并添加在这儿吧！


<docmeta name="displayName" value="Available Adapters">
