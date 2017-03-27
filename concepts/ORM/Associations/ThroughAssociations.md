# 直达的关联
### 概述
多对多的直达关联表现和多对多的关联一样，除了会自动为你创建join表之外。在一个多对多的直达关联中你定义了一个模型包含了两个你想合并一起的模型的字段。当你添加`through`关键词定义一个关联的时候，该模型应该被使用而不是自动join到表中。

### Has Many Through例子

```javascript
// myApp/api/models/User.js
module.exports = {
  attributes: {
    name: {
      type: 'string'
    },
    pets:{
      collection: 'pet',
      via: 'owner',
      through: 'petuser'
    }
  }
}
```

```javascript
// myApp/api/models/Pet.js
module.exports = {
  attributes: {
    name: {
      type: 'string'
    },
    color: {
      type: 'string'
    },
    owners:{
      collection: 'user',
      via: 'pet',
      through: 'petuser'
    }
  }
}
```

```javascript
// myApp/api/models/PetUser.js
module.exports = {
  attributes: {
    owner:{
      model:'user'
    },
    pet: {
      model: 'pet'
    }
  }
}
```

<<<<<<< HEAD
通过使用`PetUser`模型我们可以在`User`模型和`Pet`模型上都使用`.populate()`，只需要和一个正常的[Many-to-Many](http://sailsjs.org/documentation/concepts/models-and-orm/associations/many-to-many)关联一样。
=======
By using the `PetUser` model we can use `.populate()` on both the `User` model and `Pet` model just the same as a normal [Many-to-Many](http://sailsjs.com/documentation/concepts/models-and-orm/associations/many-to-many) association.
>>>>>>> upstream/master

> 当前如果你想要添加额外的信息到`through`表中，那么当地奥欧勇`.populate`的时候将不会生效。为了实现这个你需要手工地去查询`through`模型。



<docmeta name="displayName" value="Through Associations">

