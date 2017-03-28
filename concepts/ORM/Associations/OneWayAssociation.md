# 单向连接

**也就是从属于的意思**

### 概述
单向关联指的是一个模型关联到另一个模型。你可以查询这个模型并填充获取被相关模型。但是你不能查询被相关模型和通过被相关模型找到相关模型。

### 单向相关例子
在这个例子中，我们关联一个`User`到一个`Pet`，而不是一个`Pet`到一个`User`。

```javascript
// myApp/api/models/Pet.js
module.exports = {
  attributes: {
    name: {
      type: 'string'
    },
    color: {
      type: 'string'
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
    pony:{
      model: 'pet'
    }
  }
}
```

现在关联已经设置好了，你就可以populate出pony的关联：

```javascript
User.find({ name:'Mike' })
.populate('pony')
.exec(function(err, users) {

  // The users object would look something like:
  // [{
  //  name: 'Mike',
  //  age: 21,
  //  pony: {
  //    name: 'Pinkie Pie',
  //    color: 'pink',
  //    id: 5,
  //    createdAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST),
  //    updatedAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST)
  //  },
  //  createdAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
  //  updatedAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
  //  id: 1
  // }]
```

### 注意：

> 关于更细节的描述请参考[Waterline Docs](https://github.com/balderdashy/waterline-docs/blob/master/models/associations/associations.md)。

> 因为我们只有和一个模型形成关联，所以一个`Pet`可以属于多个`User`模型，并且数目不限制。如果我们需要，我们可以改变这个让一个`Pet`精确关联一个`User`并且该`User`精确关联一个`Pet`。


<docmeta name="displayName" value="One Way Association">

