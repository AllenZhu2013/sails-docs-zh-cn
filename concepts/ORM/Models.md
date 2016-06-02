# 模型
模型表示的结构数据的一个集合，总是对应于数据库中的一张表或者一个集合。模型总是通过在`api/models`文件夹下创建一个文件来定义。

```javascript
// Parrot.js
// The set of parrots registered in our app.
module.exports = {
  attributes: {
    // e.g., "Polly"
    name: {
      type: 'string'
    },

    // e.g., 3.26
    wingspan: {
      type: 'float',
      required: true
    },

    // e.g., "cm"
    wingspanUnits: {
      type: 'string',
      enum: ['cm', 'in', 'm', 'mm'],
      defaultsTo: 'cm'
    },

    // e.g., [{...}, {...}, ...]
    knownDialects: {
      collection: 'Dialect'
    }
  }
}
```

<!--

// api/models/Product.js
module.exports = {
  attributes: {
    nameOnMenu: { type: 'string' },
    price: { type: 'string' },
    percentRealMeat: { type: 'float' },
    numCalories: { type: 'integer' }
  }
}
-->

### 使用模型
模型可能可以通过我们的控制器、策略、服务、响应、测试或者在我们自定义的模型方法中访问到。在模型上有许多内建的方法可用，最重要的是查询方法：[find](http://sailsjs.org/documentation/reference/waterline/models/find.html)， [create](http://sailsjs.org/documentation/reference/waterline/models/create.html)， [update](http://sailsjs.org/documentation/reference/waterline/models/update.html)， 和 [destroy](http://sailsjs.org/documentation/reference/waterline/models/destroy.html)。 这些方法都是[异步的](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)--在后台中，Waterline会发送一条查询到数据库然后等待响应。

因此，查询方法会返回一个延迟的查询对象。为实际执行一条查询，`.exec(cb)`必须在这条延迟对象上调用，其中`cb`是一个回调函数来运行查询结束之后的操作。

Waterline也包含可选的promise支持。除了调用`.exec()`之外，还可以调用`.then()`, `.spread()`, 或者`.catch()`，这些函数都返回一个[Bluebird promise](https://github.com/petkaantonov/bluebird)。

### 模型方法
模型类方法是内置在函数模型本身去执行一段特有的任务在它的实例上(记录)。在这里你会发现熟悉的CRUD方法来执行数据库操作比如：`.create()`, `.update()`, `.destroy()`, `.find()`等。

##### 自定义模型方法
Waterline允许你在你的模型上定义你的方法。这个特性利用了Waterline模型会忽略未被承认的关键词这一事实，所以你需要注意不小心地就重写了内建的方法以及动态查找器(不要定义方法名称为“create”等操作)。自定义的模型方法在外推与一个特有的模型相关的控制器的代码最有效；也就是者允许你将代码从你的控制器中拉出来然后组成可重要的函数，能够在任何地方被调用到(也就是不依赖于`res`和`req`)。

模型方法一般都是异步的。按照惯例，异步模型方法应该是一个2阶的函数，将会接收一个对象作为它们的第一个参数(一般叫做`opt`或`options`)和一个Node回调作为第二个参数。或者，你也可以选择返回一个promise(这两种策略都是可以工作的--这是一个个人喜好问题，如果你没有个人偏好，最好坚持使用Node回调)。

一个最佳实践就是写你的静态的模型方法所以它能够接受要么是一个记录要么是它的首要关键值。对于同时操作多条记录的模型方法，你应该允许一个记录的数组或主关键值的数组能够传递进来。虽然这需要更多的时间来写,但是使你的方法更强大。并且因为你在使用这个区外推通用使用的逻辑，通常是值得额外的努。

比如:

```js
// in api/models/Monkey.js...

// Find monkeys with the same name as the specified person
findWithSameNameAsPerson: function (opts, cb) {

  var person = opts.person;

  // Before doing anything else, check if a primary key value
  // was passed in instead of a record, and if so, lookup which
  // person we're even talking about:
  (function _lookupPersonIfNecessary(afterLookup){
    // (this self-calling function is just for concise-ness)
    if (typeof person === 'object')) return afterLookup(null, person);
    Person.findOne(person).exec(afterLookup);
  })(function (err, person){
    if (err) return cb(err);
    if (!person) {
      err = new Error();
      err.message = require('util').format('Cannot find monkeys with the same name as the person w/ id=%s because that person does not exist.', person);
      err.status = 404;
      return cb(err);
    }

    Monkey.findByName(person.name)
    .exec(function (err, monkeys){
      if (err) return cb(err);
      cb(null, monkeys);
    })
  });

}
```

然后你可以：

```js
Monkey.findWithSameNameAsPerson(albus, function (err, monkeys) { ... });
// -or-
Monkey.findWithSameNameAsPerson(37, function (err, monkeys) { ... });
```

> 更多的细节，请阅读[Timothy the Monkey]()。

另外一个例子：

```javascript
// api/models/User.js
module.exports = {

  attributes: {

    name: {
      type: 'string'
    },
    enrolledIn: {
      collection: 'Course', via: 'students'
    }
  },

  /**
   * Enrolls a user in one or more courses.
   * @param  {Object}   options
   *            => courses {Array} list of course ids
   *            => id {Integer} id of the enrolling user
   * @param  {Function} cb
   */
  enroll: function (options, cb) {

    User.findOne(options.id).exec(function (err, theUser) {
      if (err) return cb(err);
      if (!theUser) return cb(new Error('User not found.'));
      theUser.enrolledIn.add(options.courses);
      theUser.save(cb);
    });
  }
};
```

#### 动态发现器
当你启动你的Sails app的时候会动态生成一些专用的静态方法。比如，如果你的Person模型有一个“firstName”，你可以这样运行：

```js
Person.findByFirstName('emma').exec(function(err,people){ ... });
```

#### 资源丰富的发布订阅方法
一个用pubsub钩子勾住的特殊类型模型方法。更多参考[section of the docs on resourceful pubsub](http://sailsjs.org/documentation/reference/websockets/resourceful-pubsub)。

<!--
another special type of class method.  It stands for 'Publish, Subscribe' and that's just what they do. These methods play a big role in how Sails integrates and utilizes Socket.IO.  They are used to subscribe clients to and publish messages about the creation, update, and destruction of models.  If you want to build real-time functionality in Sails, these will come in handy.
-->

#### 属性方法
属性方法是在记录(也就是模型实例)上可用的方法，从Waterline查询中返回。比如，如果你从Student模型中找到10个高GPA的学生，那些学生中的每一个的记录都可以访问内建的属性方法，以及任何定义在Student模型上的自定义属性方法。

###### 内建的属性方法
每一个Waterline方法自动包含一些属性方法，包括：

+ [`.toJSON()`](http://sailsjs.org/documentation/reference/waterline/records/toJSON.html)
+ [`.save()`](http://sailsjs.org/documentation/reference/waterline/records/save.html)
+ [`.destroy()`](http://sailsjs.org/documentation/reference/waterline/models/destroy.html)
+ [`.validate()`](http://sailsjs.org/documentation/reference/waterline/records/validate.html)

<!-- note to self- we should bundle a getPrimaryKeyValue() attribute method on every model in waterline core (or maybe just getId() since "id" is simpler to understand) ~mike - aug2,2014 -->

###### 自定义属性方法
Waterline模型也允许你去定义你自己的属性方法。就像别的属性一样定义它们，但是不是在一个属性定义对象上，写一个函数在右手边。

```js
// From api/models/Person.js...

module.exports = {
  attributes: {
    // Primitive attributes
    firstName: {
      type: 'string',
      defaultsTo: ''
    },
    lastName: {
      type: 'string',
      defaultsTo: ''
    },

    // Associations (aka relational attributes)
    spouse: { model: 'Person' },
    pets: { collection: 'Pet' },

    // Attribute methods
    getFullName: function (){
      return this.firstName + ' ' + this.lastName;
    },
    isMarried: function () {
      return !!this.spouse;
    },
    isEligibleForSocialSecurity: function (){
      return this.age >= 65;
    },
    encryptPassword: function () {

    }
  }
};
```

> 值得注意的除了内建的`.save()`和`.destroy()`属性方法之外，其他的属性方法几乎都是与约定*同步*。
> 也还要注意自定义的属性方法默认是不会序列化成JSON的。那么为了序列化它们，你可以重写[toJSON](http://sailsjs.org/documentation/reference/waterline-orm/records/to-json)。

###### 什么时候写一个自定义属性方法
自定义属性方法对从一个记录中提取信息特别有用。也就是说，你可能想要从一个或者多个属性中减少一些信息(也就是“这个人结婚了吗？”)。

```js
if ( rick.isMarried() ) {
  // ...
}
```

###### 什么时候**不要**写一个自定义属性方法
**你应该避免写你的异步属性方法**。虽然内建的异步属性方法像`.save()`和·`.destroy()`可以在你的app上方便地使用，但是写出你自己的异步属性方法有时候会有一些不想要的结果，并且也不是最有效地构建你的app

比如，考虑到一个app管理结婚记录。你也许认为写一个属性方法在Person模型上更新另个人的`spouse`属性在数据库。那么你的控制器代码可能是这样的：

```js
personA.marry(personB, function (err) {
  if (err) return res.negotiate(err);
  return res.ok();
})
```

貌似看着很对。。。。直到你需要写一个不同的动作但是你并没有一个实际的“PersonA”的记录。

一个最好的办法就是写一个自定义的(静态)模型方法。这会让你的函数变得更加可重用性和通用，因为无论你是否有一个实际的记录实例还是没有它都是可访问的。你也许需要重构代码如下：

```js
Person.marry([joe,raquel], function (err) {
  if (err) return res.negotiate(err);
  return res.ok();
})
```

###### 命名你的属性方法
确保你使用一个命名规则来帮助你避免将**属性方法**和*属性值*混淆当你在使用记录的时候。一个最佳实践是使用“get*”(比如`getFileName()`)前缀并避免写属性方法原状地改变记录。

<!--

Imagine you have a small monkey named Timothy that rides on your shoulders and styles your hair when you are scheduled to speak at a conference.  In this scenario, you are a record of the `Person` model and Timothy is a record of the `Monkey` model. The `Person` model has primitive attributes like "name", "email", and "phoneNumber", and relational attributes like "petMonkey" (points to an individual `Monkey`) and "mom" (points to an individual `Person`).  Meanwhile the `Monkey` model has primitive attributes "name", "age", and "demeanor", as well as an relational attribute: "petOfPerson" (which points to an individual person).


Everyone knows that a person can style her own hair, but it is more efficient if her pet monkey does it.  We can represent this by definining `styleHair: function (cb){ return cb(); }` as an attribute method on Person and `styleOwnersHair: function (cb){ return cb();}` as an attribute method on Monkey.


If your app involves multigenerational hair-styling, you might think it would make sense to write an attribute method on the Monkey model called "getOwnersGrandma()" which would call a callback with the monkey's owner's mom's mom.
-->

<!--

###### an aside about promises

Promises are most effective when used to handle asynchronous, but referentially transparent ("nullipotent") operations; i.e. logic without any side-effects.
-->





<docmeta name="displayName" value="Models">
