# 一对多
### 概述
一个一对多的连接表示的一个模型可以关联到多个模型。为了构建这种连接一个虚拟的属性被添加到一个模型中使用`collection`属性。在一个一对多的关联中一端必须有一个`collection`属性并且其他端必须包含一个`model`属性。这就允许多端知道哪个记录它们需要去获取当一个`populate`使用的时候。

因为你也许想要一个模型有多个一对多的关联在其他模型上，那么`via`的关键词在`collection`属性上是必须的了。这表明在关联的一端的`model`属性总是用于populate记录出来。

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

    // Add a reference to Pets
    pets: {
      collection: 'pet',
      via: 'owner'
    }
  }
};
```
```javascript
// myApp/api/models/Pet.js
// A pet may only belong to a single user
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
    owner: {
      model: 'user'
    }
  }
};
```

现在pets和users已经互相知道对方了，它们可以被关联了。为了实现这个关联我们可以为`owner`值创建或者更新pet的user关键词。

```javascript
Pet.create({
  breed: 'labrador',
  type: 'dog',
  name: 'fido',

  // Set the User's Primary Key to associate the Pet with the User.
  owner: 123
})
.exec(function(err, pet) {});
```

现在`Pet`已经关联到`User`了，所有的pets都从属于一个指定的用户，并使用`populate`方法找到。

```javascript
User.find()
.populate('pets')
.exec(function(err, users) {
  if(err) // handle error

  // The users object would look something like the following
  // [{
  //   id: 123,
  //   firstName: 'Foo',
  //   lastName: 'Bar',
  //   pets: [{
  //     id: 1,
  //     breed: 'labrador',
  //     type: 'dog',
  //     name: 'fido',
  //     user: 123
  //   }]
  // }]
});
```

### 注意
> 关于更细节的描述请参考[Waterline Docs](https://github.com/balderdashy/waterline-docs/blob/master/models/associations/associations.md)。



<docmeta name="displayName" value="One-to-Many">

