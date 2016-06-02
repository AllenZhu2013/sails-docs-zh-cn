# Waterline：SQL/noSQL 数据的映射器(ORM/ODM)
Sails默认安装一个非常有用的[ORM/ODM](http://stackoverflow.com/questions/12261866/what-is-the-difference-between-an-orm-and-an-odm)叫做[Waterline](https://github.com/balderdashy/waterline)，一种与数据存储无关的工具，可以动态地简化与一个或多个[数据库](http://www.cs.umb.edu/cs630/hd1.pdf)的交互。它提供了一个在底层数据库的上面的抽象层，允许你容易地查询和操作你的数据而不需要写生产商指定的集成代码。

### 数据库无关性
在schemaful的数据库比如[Postgres](http://www.postgresql.org/), [Oracle](https://www.oracle.com/database)和[MySQL](http://www.mysql.com)，模型都是用表来表示。在[MongoDB](http://www.mongodb.org)，它们是通过Mongo的"集合"来表示。在[Redis](http://redis.io)，它们使用键值对来表示。每一种数据库都有自己独特的查询特性，并且在一些情况下还需要安装和编译一个专用的本地模块来连接到服务器上。这涉及到大量的开销以及存储一个混乱的厂商锁定层([vendor lock-in](https://en.wikipedia.org/wiki/Vendor_lock-in))到一个专用的数据库；比如如果你的app使用了一大堆SQL查询，那么以后就很难切换到Mongo或者Redis，反之亦然。

Waterline查询语法则是建立在这之上，关注于业务逻辑比如创建一条新的记录，获取或者搜索存在的记录，更新记录或者删除记录。无论你使用的是什么数据库，使用方法*几乎一样*。此外，Waterline允许你去[`.populate()`](http://sailsjs.org/documentation/reference/waterline/queries/populate.html)模型之间的关联，*即使*每个模型的数据放在不同的数据库中。那意味着你可以切换你的app的模型从Mongo到Postgres，到MySQL、到Redis，然后不修改任何代码也能切换回来。有时候当你需要底层的、数据库特有功能的时候，Waterline就会提供一个查询接口允许你直接操作你模型的底层数据库驱动。(参考(参考[.query()](http://sailsjs.org/documentation/reference/waterline/models/query.html)和[.native()](http://sailsjs.org/documentation/reference/waterline/models/native.html))

### 方案
设想你正在构建一个电商网站，附带一个移动app。用户通过关键词查找产品或者通过分类浏览产品，然后购买。你的app中有些部分是很普通的；你有一个API驱动流来登录，注册和购买付款，复位密码等。但是你知道在你的技术路线中有一些不简单的特性潜伏者并且会让你的app变得复杂。果不其然：

##### 灵活性
*你问产品线他们将会使用什么数据库？*

> "数据库... 什么东东? 我们不能太草率, 否则会做出错误的选择的。我将会展开调查。让我们开始干活吧。"

传统的方法去选择一个单一的服务器给一个web应用实际上是禁止的。时常的，应用程序需要兼容性地与一个或者多个已经存在的数据组维护或者它需要因为性能的原因使用不同类型的数据库。

因为Sails默认使用`sails-disk`，所以你可以零配置的开始构建你的app，使用一个本地临时文件作为存储。当你开始切换到真正的数据库的时候，只需要改变你的app的连接配置([connection configuration](http://sailsjs.org/documentation/reference/configuration/sails-config-connections))。

##### 兼容性
*产品的所有者或者利益相关者向你走过来并说:*

> "顺便说一句呀，这个产品实际上已经在我们的销售系统中了。我猜它是某些ERP的东西，或者更像是“DB2”？无论如何，我确定你可以完成的--听起来很容易呀？"

许多企业级的应用都必须与现存的数据库进行集成。如果你够幸运的话，只需要一次数据的迁移就够了，但是大部分情况下，已经存在的数据库仍然会被应用程序改写一番。为了构建你的app，你也许需要从多个遗留系统或者存储在各种地方的独立数据组中取出数据。这些数据组可以存在全球的5个不同的离散服务器中。一个托管的数据库服务器也许覆盖一个有着关系型数据的SQL数据库，然而另外一个云服务器可能使用着Mongo或者Redis的集合。

Sails/Waterline让你的不同模型与不同的数据库关联起来；无论是本地的还是在互联网中的任意地方。你可以构建一个User模型并映射到在一个传统数据库中的一个自定义的MySQL表。同理一个Product模型也可以映射到在DB2中的一张表或者Order模型可以映射到一个MongoDb的集合。最好的是，你可以`.populate()`这些不同的数据库和适配器，所以如果你配置一个模型到一个到一个不同的数据库，你的控制器或模型的代码不再需要改变(注意的是你*需要*手工地迁移任何重要的产品数据)。

##### 性能
*你晚上坐在你的电脑前面，然后你意识到：*

> "我应该怎样做关键词搜索呢？这个产品的数据没有任何关键词，但是产品线却想要基于n-gram 单词顺序来得到搜索结果。并且我对这个推荐引擎如何工作也没有任何概念。如果我今晚不止一次听到·大数据`这个单词，那么我宁愿推出回到以前的咖啡店工作。“

所以"大数据”是指的什么呢？正常地当你听到博主或者分析者使用这个专业术语的时候，你会想到数据挖掘和商业智能。你也许想到这是一个将数据从多个源中取下来，然后处理/索引/分析它，最后将提取的信息写到任何地方并且将原始数据保留着或者丢掉。

然而，这里也有一些普遍的挑战，那就是提供他们自己给同样的索引或分析需求；像"driving-direction-closeness" 这类的搜索，或者一个相关产品的建议引擎。幸运的是，许多数据库简化了专用的大数据用例(比如MongoDb提供地理空间索引，ElasticSearch提供全文本搜索索引的支持)

用这种方法使用数据库可以提供巨大的性能优势，尤其是当涉及到复杂的报表查询、搜索、和NLP/机器学习时候。某些数据库擅长于应答传统的关系型业务查询，另外一些则适合map/reduce-style processing of data，使用最优化/trade-offs for blazing-fast read/writes。这种考虑尤其重要当你的app是基于用户可伸缩的时候。

### 适配器
就像大部分的MVC框架，Sails支持[多个数据库](http://sailsjs.org/features)。这意味着查询和操作数据的语法基本一样，无论我们使用的是MongoDb还是MySQL或者其他支持的数据库。

Waterline使用它的适配器概念灵活地构建。一个适配器是一组代码，这些代码映射方法比如`find()`和`create()`到底层语法如`SELECT * FROM`和`INSERT INTO`。Sails的内核组维护了[众多流行的数据库](http://sailsjs.org/features)的开源适配器并且大量的[社区适配器](https://github.com/balderdashy/sails-docs/blob/0.9/Database-Support.md)可用。

自定义的Waterline适配器实际上[很容易构建](https://github.com/balderdashy/sails-generate-adapter)，也更易于维护集成。 anything from a proprietary enterprise system, to an open API like LinkedIn, to a cache or traditional database.

### 连接
一个**连接**代表一个特有的数据库配置。这个配置对象包含一个被使用的适配器同时也包含象端口、主机、用户名称等信息。如果你的数据库不需要密码的话直接删掉密码就行了，连接定义在[`config/connections.js`](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html).：

```javascript
// in config/connections.js
// ...
{
  adapter: 'sails-mysql',
  host: 'localhost',
  port: 3306,
  user: 'root',
  password: 'g3tInCr4zee&stUfF'
}
// ...
```

根据正在使用的适配器，它可能使用unix 套接字，所以不需要端口和主机。下面的例子是使用一个存在的MAMP mysql 服务器和sails-mysql适配器：

```javascript
// in config/local.js
// ...
connections:{
  local_mysql:{ //arbitrary name
    module: 'sails-mysql',
    user: 'root',
    password: 'root',
    database: 'sailstest1',
    socketPath: '/Applications/MAMP/tmp/mysql/mysql.sock'
  }
}
// ...
```

另外一个使用一个url配置适配器的例子：

```javascript
// in config/local.js
// ...
connections:{
  local_mysql:{ //arbitrary name
    module: 'sails-mysql',
    url: 'mysql://root:root@localhost:3306/sailstest1'
  }
}
// ...
```

默认的数据库连接配置位于基本的模型配置文件中：`config/models.js`，但是他也可以基于每个模型进行改写通过指定一个[`connection`](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)。在[`config/local.js`](http://sailsjs.org/documentation/concepts/configuration/the-local-js-file)文件中重写连接对象也是很有用的。

### 类比
想象一个文件柜里放满了笔写的表格。所有的表格都有相同的字段(比如"name", "birthdate", "maritalStatus")，但是每一张表格中字段的值都是不同的。比如，其中一张可能包含 "Lara", "2000-03-16T21:16:15.127Z", "single"，但是另外一张可能包含："Larry", "1974-01-16T21:16:15.127Z", "married"。

现在现在想象你正在运行一个热狗业务。如果你非常有条理，你可能这样收拾你的文件柜：

+ **Employee** (包含你的员工记录)
  + `fullName`
  + `hourlyWage`
  + `phoneNumber`
+ **Location** (包含你可以操作的每个位置的记录)
  + `streetAddress`
  + `city`
  + `state`
  + `zipcode`
  + `purchases`
    + 一组在这个位置上所有的购买的东西的列表
  + `manager`
    + 管理这个位置的员工
+ **Purchase** (包含了你的客户购买的每一件东西的每一条记录)
  + `madeAtLocation`
  + `productsPurchased`
  + `createdAt`
+ **Product** (包含了每一个你提供的产品)
  + `nameOnMenu`
  + `price`
  + `numCalories`
  + `percentRealMeat`
  + `availableAt`
    + 是一组这个产品供货可用位置的列表。

在你的Sails app中，一个**模型**就类似于一个文件柜。它包含的**记录**就类似于表格。`Attribute`就是表格中的字段。

### 注意：

+ 如果你重写了内建的ORM或[Waterline](https://github.com/balderdashy/waterline)那么该文档将不再可用。  在那种情况下，你的模型将会依着你设置的会话来执行而不管你正在使用的是哪种ORM库(比如Mongoose)。




<docmeta name="displayName" value="Models and ORM">
