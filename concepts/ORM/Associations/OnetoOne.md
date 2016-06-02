# 一对一
### 概述
一对一表示的是一个模型只能和另一个模型关联。为了让一个模型知道它相关联的另外一个模型是什么，一个foreign的关键词必须包含在其中的一个记录里并且带有一个`unique`数据库来约束它。

当前在Waterline中有两种方法来操作这种关联。

### Has One Using A Collection
在这个例子中，我们关联一个`Pet`和一个`User`。`User`只能有一只`Pet`，反之亦然，一个`Pet`只能有一个`User`。然而，为了在这里中双方能够互相查询，我们必须添加一个`collection`属性到`User`模型中。这允许我们都可以调用`User.find().populate('pet')和`Pet.find().populate('owner')`。

这两种方法通过更新`Pet`模型的`owner`属性来保持同步。添加`unique`属性确保每一个`owner`只有一个值存在于数据库中。当你从`User`端populate出数据的视乎你将会得到一个数组。

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
    owner:{
      model:'user',
      unique: true
    }
  }
}
```

```javascript
// myApp/api/models/User.js
module.exports = {
  attributes: {
    name: {
      type: 'string'
    },
    age: {
      type: 'integer'
    },
    pet: {
      collection:'pet',
      via: 'owner'
    }
  }
}
```
### Has One Manual Sync
在这个例子中，我们关联一个`Pet`和一个`User`。`User`只能有一只`Pet`，反之亦然，一个`Pet`只能有一个`User`。然而，为了在这里中双方能够互相查询，我们必须添加一个`model`属性到`User`模型中。这允许我们都可以调用`User.find().populate('pet')和`Pet.find().populate('owner')`。

但是这两种模型将不会保持同步。所以当你更新其中一端的时候记得也去更新另外一端。


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
    owner:{
      model:'user'
    }
  }
}
```

```javascript
// myApp/api/models/User.js
module.exports = {
  attributes: {
    name: {
      type: 'string'
    },
    age: {
      type: 'integer'
    },
    pet: {
      model:'pet'
    }
  }
}
```


###注意
> 关于更细节的描述请参考[Waterline Docs](https://github.com/balderdashy/waterline-docs/blob/master/models/associations/associations.md)。



<docmeta name="displayName" value="One-to-One">

