# 生命周期回调
### 概述
生命周期回调是能够在某些*模型*动作之前或之后自动调用的功能。比如，我们有时使用生命周期回调自动化地加密一个密码在创建或者更新一个`Account`模型之前。

Sails默认暴露了一个有用的生命周期调用。

##### Callbacks on `create`

  - beforeValidate: fn(values, cb)
  - afterValidate: fn(values, cb)
  - beforeCreate: fn(values, cb)
  - afterCreate: fn(newlyInsertedRecord, cb)


##### Callbacks on `update`

  - beforeValidate: fn(valuesToUpdate, cb)
  - afterValidate: fn(valuesToUpdate, cb)
  - beforeUpdate: fn(valuesToUpdate, cb)
  - afterUpdate: fn(updatedRecord, cb)

##### Callbacks on `destroy`

  - beforeDestroy: fn(criteria, cb)
  - afterDestroy: fn(destroyedRecords, cb)


### 例子
如果你想在保存到数据库之前加密一个密码，你也许会使用`beforeCreate`生命周期回调。

```javascript
var bcrypt = require('bcrypt');

module.exports = {

  attributes: {

    username: {
      type: 'string',
      required: true
    },

    password: {
      type: 'string',
      minLength: 6,
      required: true,
      columnName: 'encrypted_password'
    }

  },


  // Lifecycle Callbacks
  beforeCreate: function (values, cb) {

    // Encrypt password
    bcrypt.hash(values.password, 10, function(err, hash) {
      if(err) return cb(err);
      values.password = hash;
      //calling cb() with an argument returns an error. Useful for canceling the entire operation if some criteria fails.
      cb();
    });
  }
};
```



<docmeta name="displayName" value="Lifecycle callbacks">
