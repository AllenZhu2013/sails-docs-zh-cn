# Associations
使用Sails和Waterline的时候，你可以关联模型到多个数据存储中。这意味着即使你的users存储在[PostgreSQL](http://www.postgresql.org/)和你的照片放在[MongoDb](http://www.mongodb.com/)，你也可以交互这些数据就好像它们是在同一个数据库一样。你也可以使用相同的适配器关联到不同范围的[连接](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)(也就是数据存储或数据库)。这样很方便如果比如你的app需要访问或者更新传统的存储在一个[MySQL](http://www.mysql.com/)中的食谱数据在你的公司的数据中心，而且还可以从一个云上的一个新的MySQL数据中存储和获取配料数据。

# 支配
## 例子本体

```javascript
// User.js
module.exports = {
  connection: 'ourMySQL',
  attributes: {
    email: 'string',
    wishlist: {
      collection: 'product',
      via: 'wishlistedBy'
    }
  }
};
```


```javascript
// Product.js
module.exports = {
  connection: 'ourRedis',
  attributes: {
    name: 'string',
    wishlistedBy: {
      collection: 'user',
      via: 'wishlist'
    }
  }
};
```

### 问题描述
很容易看出在这个跨适配器关系中发生了什么。在Users和Products之间是一个多对多(`N->...`)的关系。实际上，你可以想象一些其他的关系存在着，但是因为可能使用一个中间人模型更好描述，所以我在这个例子中用了简单的实例。

无论如何，这总是最好的...但是资源的关系在哪儿？"ProductUser"，如果你能够原谅面向SQL的命令方法。我们知道这最终会在这一端或者另一端结束，但是如果我们想控制它在哪个数据库结束呢？

> **IMPORTANT NOTE**
>
>*这只是一个问题因为关联的双方都有一个`via`修饰符指定！！*如果缺少via，集合的属性总是表现为`dominant: true`。参考下面的FAQ。

## 解决方案
终于，它甚至可能指定一个第三方的连接/适配器来用在这个join表中。目前，我们关注在选择一段还是另一端。

我们解决这个问题通过“主导地位”这个概念。在任何跨适配器的关系中，其中一段被假设为主导方，如果将其类推到一个有多个国家的父母的孩子必须选择一个国家或者其他国家作为他的[公民身份](http://en.wikipedia.org/wiki/Japanese_nationality_law)来想更好理解。

下面是另一个本体例子，但是这次我们只是MySQL数据库为“主导地位”，这意味着“ProductUser”关系“表”将会作为一张SQL表存储。

```javascript
// User.js
module.exports = {
  connection: 'ourMySQL',
  attributes: {
    email: 'string',
    wishlist: {
      collection: 'product',
      via: 'wishlistedBy',
      dominant: true
    }
  }
};
```


```javascript
// Product.js
module.exports = {
  connection: 'ourRedis',
  attributes: {
    name: 'string',
    wishlistedBy: {
      collection: 'user',
      via: 'wishlist'
    }
  }
};
```

## 选择一个主导方
有一些因素会影响你的决定：
+ 如果其中一端是一个SQL数据库，那么将关系表放在这端将会让你的查询更加有效率，因为在其他端与之通信的时候关系型表可以连接起来。这就将总共的查询次数从3降到2了。
+ 如果一个连接快于另外一个连接，其他东西都是一样的话，把连接放在那一端将会更有意义。
+ 如果你知道迁移其中的一个连接更容易，你也许应该设置那一端为`主导方`。类似的，规则或者兼容问题也会影响你的决定。如果那关系包含着敏感的病人信息(比如在`Patient`和`Medicine`之间的关系)你想要确保所有相关的数据被保存在另外一个特有的数据库中(在这个例子中，`Patient`更像是`主导方`)
+ 沿着相同的路线，如果你的连接中有一个是只读的(也许在上一个例子中的`Medicine`连接到一个只读的生产商的数据库)，你不能够写数据到该数据库，所以你确保你的关系数据能够在另外一端保证安全。

## FAQ

##### 如果其中的一个连接没有via咋办？
 > 如果关联的一个`连接`没有`via`属性，那么它会自动被设为`dominant: true`
 
##### 如果两个连接都没有via属性？
> 如果两个`连接`都没有`via`属性，那么它们就没有相关性。二者都是`主导方`因为它们在独立的关系表中。

#####模型关联怎么样？ 
> 在其他类型的连接中，`dominant`属性是禁用的。设置一端为`dominant`只在关联两个都有一个属性`{ via: '...', collection: '...' }`的模型的时候才有必要。

##### 可以一个模型因为某个属性成为主导方而另外一个不是？
> 请记住一个模型成为“主导”只发生在一种特有关系的环境中。一个模型也许会在一个或多个关系中成为主导但同时地不会再其他的关系中成为主导。
> 比如如果一个`User`有一个toys的集合叫做`favoriteToys`通过在`Toy`模型中的`favoriteToyOf`，并且在`User`的`favoriteToys`是`dominant: true`，`Toy`可以在别的方法上成为主导。所以`Toy`可能关联到User通过它的属性方法，`designedBy``，`dominant: true`。


##### 两个模型都可以成为主导吗？
> 不。如果双方的模型都在一个跨适配器或跨连接的话，多对多连接的时候设置`dominant: true`，将会在启动的时候报错。

##### 模型中可以都不是dominant吗？
> 在某种程度上，如果在一个跨适配器或者跨连接的多对多关联中没有设置模型为`dominant:true`，那么在启动服务器之前会显示一条警告，并且系统会基于当前关系的特征自动地设置。到目前为止，这仅仅是意味着基于字母顺序做的一个随便的决定而已。

##### 关于没有跨适配器的连接呢？

> `dominant`属性在非跨适配器/跨连接的关联中是自动忽略掉的。我们假设你也许最后计划在多连接的时候瓦解数据，那么就没有理由阻止你提前这样做，另外，这个也保留了额外的`dominant`选项的未来可用性。



<docmeta name="displayName" value="Dominance">

