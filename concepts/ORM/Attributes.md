# 属性值
### 概述
模型的属性是一个模型的基本的信息。一个Person的模型可能有属性叫做`firstName`,`lastName`,`phoneNumber`,`age`,`birthDate`和` emailAddress`。

### 属性选项
这些选项可以用来执行各种约束条件并且添加特殊的增强功能到我们的模型中。

##### Type
指定该属性存储的数据类型，可以使如下的一种：

- string
- text
- integer
- float
- date
- datetime
- boolean
- binary
- array
- json
- mediumtext
- longtext
- objectid

##### defaultsTo
当创建一条记录的时候，如果没有设置任何值，该记录将会创建一个`defaultsTo`指定的值：

```javascript
attributes: {
  phoneNumber: {
    type: 'string',
    defaultsTo: '111-222-3333'
  },
  orderNumber: {
    type: 'text',
    defaultsTo: function() {
      return uuid.v4();
    }
  }
}
```

##### autoIncrement
设置该属性是一个自动递增的关键词。当一条记录被添加到模型中去，如果该属性的值没有指定，它将会根据最近的记录的值递增加1.注意：指定`autoIncrement`的属性的数据类型应该是`type: 'integer'`。同时请牢记于心：在不同的数据存储支持的程度各有不同。比如，MySQL不允许每张表多余一个的自动递增列。

```javascript
attributes: {
  placeInLine: {
    type: 'integer',
    autoIncrement: true
  }
}
```

##### unique
确保目标属性没有出现两条记录有同样的值。这是一个适配器层的约束条件，所以在大部分情况下这会让该属性成为一个唯一的索引在底层数据库创建的时候。

```javascript
attributes: {
  username: {
    type: 'string',
    unique: true
  }
}
```

> 当使用带有`utf8mb4`字符集的MySQL，你会需要通过MySQL直接在你的表中添加 `size`约束条件到合适的列中。否则，因为在MySQL适配器中`type: 'string'`会被转换为`varchar(255)`，所以`unique: true`的约束条件将会导致一个'index too long'的错误: `ER_INDEX_COLUMN_TOO_LONG: Index column size too large. The maximum column size is 767 bytes.`


<<<<<<< HEAD
##### primaryKey
使用了这个那么该记录的这条属性就会成为主要关键词。一条模型中只能有一个属性是`primaryKey`。注意：除非[autoPK](http://sailsjs.org/documentation/concepts/ORM/model-settings.html?q=autopk)设置为false，否则不要去使用该关键词。
=======
###### index

Will create a simple index in the underlying datastore for faster queries if available. This is only for simple indexes and currently dosn't support compound indexes. For these you will need to create them yourself or use a migration.

There is currently an issue with adding indexes to string fields. Because Waterline performs its queries in a case insensitive manner we are unable to use the index on a string attribute. There are some workarounds being discussed but nothing is implemented so far. This will be updated in the near future to fully support indexes on strings.

javascript
attributes: {
  email: {
    type: 'string',
    index: true
  }
}

-->

###### primaryKey

Use this attribute as the the primary key for the record. Only one attribute per model can be the `primaryKey`.  Note: This should never be used unless [autoPK](http://sailsjs.com/documentation/concepts/ORM/model-settings.html?q=autopk) is set to false.
>>>>>>> upstream/master

```javascript
attributes: {
  uuid: {
    type: 'string',
    primaryKey: true,
    required: true
  }
}
```

##### enum
一个特殊的属性验证，也就是只会保存那些匹配白名单中的那些值。

```javascript
attributes: {
  state: {
    type: 'string',
    enum: ['pending', 'approved', 'denied']
  }
}
```

<!--
These are not ready for prime-time yet, but listing them here so they're easy to reference and add to official docs later:

###### example

An example value for this attribute, e.g. "Albus Dumbledore".


###### validationMessage

A custom validation message to use when any validations fail for this attribute.

-->

##### size
如果在适配器中支持，那么可以用来定义属性的长度，比如在MySQL，size可以指定为一个数字来创建一个带有SQL 数据类型`varchar(n)`的一列。

```javascript
attributes: {
  name: {
    type: 'string',
    size: 24
  }
}
```

##### columnName
在一个属性内部定义中，你可以指定一个`columnName`来强制让Sails或Waterline来存储该属性在一个配置的集合中一个指定列的数据。我们知道这在SQL-特有中不是必须的--它也可以工作在MongoDb等。

当`columnName`属性主要设计为与已经存在的或者是传统的数据库工作，它也可以在你的数据库和其他应用共共享的时候这种情况下有用，或者你没有权限改变schema。

为了存储或者获取你的模型的`numberOfWheels`属性到/从`number_of_round_rotating_things`列中：

```javascript
  // An attribute in one of your models:
  // ...
  numberOfWheels: {
    type: 'integer',
    columnName: 'number_of_round_rotating_things'
  }
  // ...
```

接下去是有更形象的例子。
让我们假设你有一个`User`模型在你的Sails app中，如下：

```javascript
// api/models/User.js
module.exports = {
  connection: 'shinyNewMySQLDatabase',
  attributes: {
    name: {
      type: 'string'
    },
    password: {
      type: 'string'
    },
    email: {
      type: 'email',
      unique: true
    }
  }
};
```

所有的一切都工作得很好除了使用一个已经存在的MySQL数据库放在一个你想存放你想要的用户的地方：

```javascript
// config/connections.js
module.exports = {
  // ...

  // Existing users are in here!
  rustyOldMySQLDatabase: {
    adapter: 'sails-mysql',
    user: 'bofh',
    host: 'db.eleven.sameness.foo',
    password: 'Gh19R!?had9gzQ#Q#Q#%AdsghaDABAMR>##G<ADMBOVRH@)$(HTOADG!GNADSGADSGNBI@(',
    database: 'jonas'
  },
  // ...
};
```

让我们假设这里有一张叫做`our_users`的表放在旧的MySQL数据库，如下：

| the_primary_key | email_address | full_name | seriously_encrypted_password|
|------|---|----|---|
| 7 | mike@sameness.foo | Mike McNeil | ranchdressing |
| 14 | nick@sameness.foo | Nick Crumrine | thousandisland |


为了从Sails中使用它，你需要改变你的`User`模型成这样：

```javascript
// api/models/User.js
module.exports = {
  connection: 'rustyOldMySQLDatabase',
  tableName: 'our_users',
  attributes: {
    id: {
      type: 'integer',
      unique: true,
      primaryKey: true,
      columnName: 'the_primary_key'
    },
    name: {
      type: 'string',
      columnName: 'full_name'
    },
    password: {
      type: 'string',
      columnName: 'seriously_encrypted_password'
    },
    email: {
      type: 'email',
      unique: true,
      columnName: 'email_address'
    }
  }
};
```

<<<<<<< HEAD
> 你也许注意到了我们在这个例子中也使用了[`tableName`](http://sailsjs.org/documentation/concepts/ORM/model-settings.html?q=tablename)属性。这允许我们去控制将会用于放置我们的数据的表的名字。
=======
> You might have noticed that we also used the [`tableName`](http://sailsjs.com/documentation/concepts/ORM/model-settings.html?q=tablename) property in this example.  This allows us to control the name of the table that will be used to house our data.

>>>>>>> upstream/master







<docmeta name="displayName" value="Attributes">
