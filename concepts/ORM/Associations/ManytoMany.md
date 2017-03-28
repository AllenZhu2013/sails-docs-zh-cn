# 多对多
### 概述
一个多对多的关联表示的是一个模型可以关联到其他许多模型反之亦然。因为二者模型都能过有许多相关的模型，所以一个新的job表需要被创建来追朔这些关系。

Waterline将会查找你的模型并且如果它找到两个都有相同集合属性并分别指向对方的模型，那么它会自动构建一张join表为你。

因为你也许想要一个模型有着多个多对多的关联到另外一个模型，所以`via`关键词在`collection`属性上是必须的。这表明在关联的一端的`model`属性总是用于populate记录出来。

使用`User`和`Pet`作为例子，让我们看看是如何建立一张`User`有着许多`Pet`并且`Pet`有这许多主人。

```javascript
// myApp/api/models/User.js
// A user may have many pets
module.exports = {
  attributes: {
    firstName: {
      type: 'string'
    },
    lastName: {
      type: 'string'
    },

    // Add a reference to Pet
    pets: {
      collection: 'pet',
      via: 'owners',
      dominant: true
    }
  }
};
```
```javascript
// myApp/api/models/Pet.js
// A pet may have many owners
module.exports = {
  attributes: {
    breed: {
      type: 'string'
    },
    type: {
      type: 'string'
    },
    name: {
      type: 'string'
    },

    // Add a reference to User
    owners: {
      collection: 'user',
      via: 'pets'
    }
  }
};
```

现在`User`和`Pet`模型都已经创建并且join表也已经自动设置，我们就可以开始关联记录然后查询join表。为了实现这个让我们添加一个`User`和`Pet`然后将它们关联一起。

当使用一个多对多的关联的时候有两种方法创建关联。你可以关联两个已经存在的记录到一起或者你也可以关联一个新的记录到已存在的记录中去。为了展示这个是如何实现的我们将会介绍连接到一个`collection`属性的特殊方法：`add`和`remove`。

这些方法都是同步的方法也就是说它们在一个实例被保存的时候回自动形成一组操作队列来运行。如果在一个首要关键词使用在一个`add`上的值，在join表上将会创建一条新的记录并链接到当前模型中首要关键词指定的记录上。然而如果早一个`add`上使用一个对象作为其值，那么会创建一个新的模型然后那个模型的首要关键词将会用在新的join表的记录。你也可以使用一个以前值的数组。

> 当使用`.save()`一个populate调用，将会返回新的保存的数据给你。如果你不想让这个发生你可以使用一个在`.save()`命令中的可选的配置标志位。比如：`.save({ populate: false }, function(err) {})`。


## When Both Records Exist
```javascript
// Given a User with ID 2 and a Pet with ID 20

User.findOne(2).exec(function(err, user) {
  if(err) // handle error

  // Queue up a record to be inserted into the join table
  user.pets.add(20);

  // Save the user, creating the new associations in the join table
  user.save(function(err) {});
});
```

## With A New Record

```javascript
User.findOne(2).exec(function(err, user) {
  if(err) // handle error

  // Queue up a new pet to be added and a record to be created in the join table
  user.pets.add({ breed: 'labrador', type: 'dog', name: 'fido' });

  // Save the user, creating the new pet and associations in the join table
  user.save(function(err) {});
});
```

##  With An Array of Existing Records

```javascript
// Given a User with ID 2 and a Pet with ID 20, 24, 31

User.findOne(2).exec(function(err, user) {
  if(err) // handle error

  // Queue up a record to be inserted into the join table
  user.pets.add([ 20, 24, 31 ]);

  // Save the user, creating the new pet and associations in the join table
  user.save(function(err) {});
});
```

使用`remove`方法删除关联是很简单的。它和`add`方法的工作方式一样除了它只能接收首要关键词作为它的值之外。下面两种方法也可以一起使用：

```javascript
User.findOne(2).exec(function(err, user) {
  if(err) // handle error

  // Queue up a new pet to be added and a record to be created in the join table
  user.pets.add({ breed: 'labrador', type: 'dog', name: 'fido' });

  // Queue up a join table record to remove
  user.pets.remove(22);

  // Save the user, creating the new pet and syncing the associations in the join table
  user.save(function(err) {});
});
```

### 注意
> 关于更细节的描述请参考[Waterline Docs](https://github.com/balderdashy/waterline-docs/blob/master/models/associations/associations.md)。


<docmeta name="displayName" value="Many-to-Many">

