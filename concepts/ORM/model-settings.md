# 模型的设置
下面的这些属性可以在你的模型的顶层中指定来重写那些默认的特有模型。为了重写那些被模型共享的默认设置，编辑`config/models.js`。

### `迁移`
```javascript
migrate: 'safe'
```

简言之，这个设置控制了是否或者怎样Sails尝试使用你的schema自动构建表/集合/子集等等。

在产品环境系Sails总是使用`migrate:"safe"`来保护那些你不小心删除的数据。在开发阶段，你有以下选项可以选择：

1. safe - never auto-migrate my database(s). I will do it myself (by hand)
2. alter - auto-migrate, but attempt to keep my existing data (experimental)
3. drop - wipe/drop ALL my data and rebuild models every time I lift Sails

当你的Sails运行起来的话，Waterline验证你的数据库数据的有效性。这个标志位将会告诉Waterline做什么当数据发生损坏的时候。你可以设置这个标志位为`safe`，那么将会忽略这些损坏的数据然后继续加载。你也可以设置它为：

| Auto-Migration Strategy  | Description |
|-------------|----------------------------------------------|
|`safe`       | never auto-migrate my database(s). I will do it myself, by hand.
|`alter`      | auto-migrate columns/fields, but attempt to keep my existing data (experimental)
|`drop`       | wipe/drop ALL my data and rebuild models every time I lift Sails

> 注意，如果使用`drop`或者`alter`，你将冒着丢失你的数据的风险。小心点。千万不要使用`drop`和`alert`在你的产品数据库中。另外，在一个大的数据库，`alert`也许会花费很长的时间完成初始化，这也许会导致像`sails console`的命令看起来像挂起来一样。


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

配置这个模型将会从哪个数据库[connection](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)中获取和保存数据。默认是`localDishDb`，默认值使用`sails-disk`适配器。

### `identity`
```javascript
identity: 'purchase'
```

模型的唯一小写的关键词。比如`user`、默认一个模型的`identity`通过它的小写文件名称来自动推断，在你的模型中最好不要修改它。

### `globalId`
```javascript
globalId: 'Purchase'
```

这个标志位改变你可以访问你的模型的全局名称(如果模型的全局化使能)。你千万不要再你的模型中改变这个属性。入股想要禁用全局功能，参考[`sails.config.globals`](http://sailsjs.org/documentation/concepts/Globals?q=disabling-globals)。


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
如果设置为`false，那么会自动地禁用掉你的模型中`updatedAt`的属性的定义。默认情况下一条记录更新的时候会使用当前的时间戳自动地一条`updatedAt`属性。如果设置为一个字符串，那么对于`updatedAt`属性那个字符串将会被作为自定义字段/列名称。

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

See [Attributes](http://sailsjs.org/documentation/concepts/ORM/Attributes.html).




<docmeta name="displayName" value="Model Settings">
