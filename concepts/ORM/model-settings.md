<<<<<<< HEAD
# 模型的设置
下面的这些属性可以在你的模型的顶层中指定来重写那些默认的特有模型。为了重写那些被模型共享的默认设置，编辑`config/models.js`。
=======
# Model Settings

In Sails, the top-level properties of model definitions are called **model settings**.  This includes everything from [attribute definitions](http://sailsjs.com/documentation/concepts/models-and-orm/model-settings#?attributes), to the [database settings](http://sailsjs.com/documentation/concepts/models-and-orm/model-settings#?connection) the model will use, as well as a number of other options.

The majority of this page is devoted to a complete tour of the model settings supported by Sails.  But before we begin, let's look at how to actually apply these settings in a Sails app.


### Overview

Model settings allow you to customize the behavior of the models in your Sails app.  They can be specified on a per-model basis by setting top-level properties in a [model definition](http://sailsjs.com/documentation/concepts/models-and-orm/models), or as app-wide defaults in [`sails.config.models`](http://sailsjs.com/documentation/reference/configuration/sails-config-models).

##### Changing default model settings

To modify the [default model settings](http://sailsjs.com/documentation/reference/configuration/sails-config-models) shared by all of the models in your app, edit [`config/models.js`](http://sailsjs.com/documentation/anatomy/my-app/config/models-js).

For example, if you edit `config/models.js` so that it contains `connection: 'somePostgresqlDb'`, then, assuming you've defined a connection named `somePostgresqlDb`, you'll set PostgreSQL as your default database.  In other words, unless overridden, all of your app's models will use that PostgreSQL datastore any time built-in model methods like `.create()` or `.find()` are executed.


##### Overriding settings for a particular model

To further customize these settings for a particular model, you can specify them as top-level properties in that model's definition file (e.g. `api/models/User.js`).  This will override default model settings with the same name.

For example, if you add `autoUpdatedAt: false` to one of your model definitions (`api/models/UploadedFile.js`), then that model will no longer have an implicit `updatedAt` attribute.  But the rest of your models will be unaffected; they will still use the default setting (which is `autoUpdatedAt: true`, unless you've changed it).


##### Which approach should I use?

By convention, attribute definitions are specified in individual model files.  Most other model settings, like `schema` and `connection`, should be specified app-wide unless you need to override them for a particular model; for example, if your default datastore is PostgreSQL, but you have an `CachedBloodworkReport` model that you want to live in Redis.

Now that you know what model settings are in general, and how to configure them, let's run through and have a look at each one.

--------------------


### `migrate`
>>>>>>> upstream/master

### `迁移`
```javascript
migrate: 'safe'
```

<<<<<<< HEAD
简言之，这个设置控制了是否或者怎样Sails尝试使用你的schema自动构建表/集合/子集等等。

在产品环境系Sails总是使用`migrate:"safe"`来保护那些你不小心删除的数据。在开发阶段，你有以下选项可以选择：

1. safe - never auto-migrate my database(s). I will do it myself (by hand)
2. alter - auto-migrate, but attempt to keep my existing data (experimental)
3. drop - wipe/drop ALL my data and rebuild models every time I lift Sails
=======
The `migrate` setting controls the **auto-migration strategy** that Sails will run every time your app loads.  In short, this tells Sails whether or not you'd like it to attempt to automatically rebuild the tables/collections/sets/etc. in your database(s).

##### Database migrations

In the course of developing an app, you will almost always need to make at least one or two **breaking changes** to the structure of your database.  Exactly _what_ constitutes a "breaking change" depends on the database you're using:  For example, imagine you add a new attribute to one of your model definitions.  If that model is configured to use MongoDB, then this is no big deal; you can keep developing as if nothing happened.  But if that model is configured to use MySQL, then there is an extra step: a column must be added to the corresponding table (otherwise model methods like `.create()` will stop working.)  So for a model using MySQL, adding an attribute is a breaking change to the database schema.

> Even if all of your models use MongoDB, there are still some breaking schema changes to watch out for.  For example, if you add `unique: true` to one of your attributes, a [unique index](https://docs.mongodb.com/manual/core/index-unique/) must be created in MongoDB.


In Sails, there are two different modes of operation when it comes to [database migrations](https://en.wikipedia.org/wiki/Schema_migration):

1. **Manual migrations** - The art of updating your database tables/collections/sets/etc. by hand.  For example, writing a SQL query to [add a new column](http://dev.mysql.com/doc/refman/5.7/en/alter-table.html), or sending a [Mongo command to create a unique index](https://docs.mongodb.com/manual/core/index-unique/).  If the database contains data you care about (in production, for example), you must carefully consider whether that data needs to change to fit the new schema, and, if necessary, write scripts to migrate it.  A [number of](https://www.npmjs.com/package/sails-migrations) great [open-source tools](http://knexjs.org/#Migrations-CLI) exist for managing manual migration scripts, as well as hosted products like the [database migration service on AWS](https://aws.amazon.com/blogs/aws/aws-database-migration-service/).
2. **Auto-migrations** - A convenient, built-in feature in Sails that allows you to make iterative changes to your model definitions during development, without worrying about the reprecussions.  Auto-migrations should _never_ be enabled when connecting to a database with data you care about.  Instead, use auto-migrations with fake data, or with cached data that you can easily recreate.


Whenever you need to apply breaking changes to your _production database_, you should use manual database migrations. But otherwise, when you're developing on your laptop, or running your automated tests, auto-migrations can save you tons of time.


##### How auto-migrations work

When you lift your Sails app in a development environment (e.g. running `sails lift` in a brand new Sails app), the configured auto-migration strategy will run.  If you are using `migrate: 'safe'`, then nothing extra will happen at all.  But if you are using `drop` or `alter`, Sails will load every record in your development database into memory, then drop and recreate the physical layer representation of the data (i.e. tables/collections/sets/etc.)  This allows any breaking changes you've made in your model definitions, lik removing a uniqueness constraint, to be automatically applied to your development database.  Finally, if you are using `alter`, Sails will then attempt to re-seed the freshly generated tables/collections/sets with the records it saved earlier.  


| Auto-migration strategy  | Description |
|:-------------------------|:---------------------------------------------|
|`safe`                    | never auto-migrate my database(s). I will do it myself, by hand.
|`alter`                   | auto-migrate columns/fields, but attempt to keep my existing data (experimental)
|`drop`                    | wipe/drop ALL my data and rebuild models every time I lift Sails


##### Can I use auto-migrations in production?

The `drop` and `alter` auto-migration strategies in Sails exist as a feature for your convenience during development, and when running automated tests.  **They are not designed to be used with data you care about.**  Please take care to never use `drop` or `alter` with a production dataset.  In fact, as a failsafe to help protect you from doing this inadvertently, any time you lift your app [in a production environment](http://sailsjs.com/documentation/reference/configuration/sails-config#?sailsconfigenvironment), Sails _always_ uses `migrate: 'safe'`, no matter what you have configured.

In many cases, hosting providers automatically set the `NODE_ENV` environment variable to "production" when they detect a Node.js app.  Even so, please don't rely only on that failsafe, and take the usual precautions to keep your users' data safe.  Any time you connect Sails (or any other tool or framework) to a database with pre-existing production data, **do a dry run**.  Especially the very first time.  Production data is sensitive, valuable, and in many cases irreplaceable.  Customers, users, and their lawyers are not cool with it getting flushed.

As a best practice, make sure to never lift or [deploy](http://sailsjs.com/documentation/concepts/deployment) your app with production database credentials unless you are 100% sure you are running in a production environment.  A popular approach for solving this organization-wide is simply to _never_ push up production database credentials to your source code repository in the first place, and instead relying on [environment variables](http://sailsjs.com/documentation/reference/configuration) for all sensitive credentials.  (This is an especially good idea if your app is subject to regulatory requirements, or if a large number of people have access to your code base.) 
>>>>>>> upstream/master

当你的Sails运行起来的话，Waterline验证你的数据库数据的有效性。这个标志位将会告诉Waterline做什么当数据发生损坏的时候。你可以设置这个标志位为`safe`，那么将会忽略这些损坏的数据然后继续加载。你也可以设置它为：

##### Are auto-migrations slow?

<<<<<<< HEAD
> 注意，如果使用`drop`或者`alter`，你将冒着丢失你的数据的风险。小心点。千万不要使用`drop`和`alert`在你的产品数据库中。另外，在一个大的数据库，`alert`也许会花费很长的时间完成初始化，这也许会导致像`sails console`的命令看起来像挂起来一样。
=======
If you are working with a relatively large amount of development/test data, the `alter` auto-migration strategy may take a long time to complete at startup.  If you notice that a command like `npm test`, `sails console`, or `sails lift` appears to hang, consider decreasing the size of your development dataset.  (Remember: Sails auto-migrations should only be used on your local laptop/desktop computer, and only with small, development datasets.)

>>>>>>> upstream/master


###  `schema`
```javascript
schema: true
```

该标志位用于指示在数据库中是否支持无模式的数据库还是有模式的数据库。如果关掉，这将会允许你在一个记录中存储任意的数据，如果打开，值与定义在模型中的属性对象才会被存储。

对于一个不要求有纲目的适配器，比如MongoDb或Redis，默认的设置是false。


### `connection`
```javascript
connection: 'my-local-postgresql'
```

<<<<<<< HEAD
配置这个模型将会从哪个数据库[connection](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)中获取和保存数据。默认是`localDishDb`，默认值使用`sails-disk`适配器。
=======
The configured database [connection](http://sailsjs.com/documentation/reference/sails.config/sails.config.connections.html) where this model will fetch and save its data.  Defaults to `localDiskDb`, the default connection that uses the `sails-disk` adapter.

>>>>>>> upstream/master

### `identity`
```javascript
identity: 'purchase'
```

模型的唯一小写的关键词。比如`user`、默认一个模型的`identity`通过它的小写文件名称来自动推断，在你的模型中最好不要修改它。

### `globalId`
```javascript
globalId: 'Purchase'
```

<<<<<<< HEAD
这个标志位改变你可以访问你的模型的全局名称(如果模型的全局化使能)。你千万不要再你的模型中改变这个属性。入股想要禁用全局功能，参考[`sails.config.globals`](http://sailsjs.org/documentation/concepts/Globals?q=disabling-globals)。
=======
This flag changes the global name by which you can access your model (if the globalization of models is enabled).  You should never change this property on your models. To disable globals, see [`sails.config.globals`](http://sailsjs.com/documentation/concepts/Globals?q=disabling-globals).


>>>>>>> upstream/master


### `autoPK`
```javascript
autoPK: true
```

这个标志位触发在你的模型中主要关键词的自动定义。这个默认的PK的细节在各个适配器之间是不同(比如MySQL使用一个自动递加的整形主关键词，然而MongoDb使用一个随机的字符串UUID)。在其他情况下，主要的关键词有autoPK生成的都是唯一的。如果关闭，那么将没有主关键词生成，并且你将自己需要手工定义一个。比如：

```js
attributes: {
  sku: {
    type: 'string',
    primaryKey: true,
    unique: true
  }
}
```
### `autoCreatedAt`

```javascript
autoCreatedAt: true
```

如果设置为`false，那么会自动地禁用掉你的模型中`createdAt`的属性的定义。默认情况下一条新记录创建的时候会使用当前的时间戳自动地一条`createdAt`属性。如果设置为一个字符串，那么对于`createdAt`属性那个字符串将会被作为自定义字段/列名称。

### `autoUpdatedAt`
```javascript
autoUpdatedAt: true
```
<<<<<<< HEAD
如果设置为`false，那么会自动地禁用掉你的模型中`updatedAt`的属性的定义。默认情况下一条记录更新的时候会使用当前的时间戳自动地一条`updatedAt`属性。如果设置为一个字符串，那么对于`updatedAt`属性那个字符串将会被作为自定义字段/列名称。
=======
If set to `false`, this disables the automatic definition of an `updatedAt` attribute in your model.  By default, `updatedAt` is an attribute which will be automatically set with the current (timezone-agnostic) timestamp every time a record is updated.  If set to a string, that string will be used as the custom field/column name for the `updatedAt` attribute.
>>>>>>> upstream/master

### `tableName`

```javascript
tableName: 'some_preexisting_table'
```

你可以在你的适配器中通过添加一个`tableName`的属性来为物理集合自定义一个名字。**这不仅仅是只用于表**。在MySQL、PostgreSQL、Oracle等，这个设置引用表的名称，但是在MongoDb或Redis中它将引用集合的名字，等等。如果美誉tableName指定，Waterline将会使用模型的`identity`作为它的`tableName`。

这对工作在已经存在或者传统的数据库特别有效！

###  `attributes`

```js
attributes: {
  name: { type: 'string' },
  email: { type: 'email' },
  age: { type: 'integer' }
}
```
参考[Attributes](http://sailsjs.org/documentation/concepts/ORM/Attributes.html)。

See [Attributes](http://sailsjs.com/documentation/concepts/ORM/Attributes.html).




<docmeta name="displayName" value="Model Settings">
