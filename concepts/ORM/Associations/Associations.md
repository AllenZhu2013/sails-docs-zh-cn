# Associations
<<<<<<< HEAD
使用Sails和Waterline的时候，你可以关联模型到多个数据存储中。这意味着即使你的users存储在[PostgreSQL](http://www.postgresql.org/)和你的照片放在[MongoDb](http://www.mongodb.com/)，你也可以交互这些数据就好像它们是在同一个数据库一样。你也可以使用相同的适配器关联到不同范围的[连接](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)(也就是数据存储或数据库)。这样很方便如果比如你的app需要访问或者更新传统的存储在一个[MySQL](http://www.mysql.com/)中的食谱数据在你的公司的数据中心，而且还可以从一个云上的一个新的MySQL数据中存储和获取配料数据。
=======

With Sails and Waterline, you can associate models across multiple data stores. This means that even if your users live in [PostgreSQL](http://www.postgresql.org/) and their photos live in [MongoDB](http://www.mongodb.com/), you can interact with the data as if they lived together in the same database. You can also have associations that span different [connections](http://sailsjs.com/documentation/reference/sails.config/sails.config.connections.html) (i.e. datastores/databases) using the same adapter.  This comes in handy if, for example, your app needs to access/update legacy recipe data stored in a [MySQL](http://www.mysql.com/) database in your company's data center, but also store/retrieve ingredient data from a brand new MySQL database in the cloud.
>>>>>>> upstream/master

> **IMPORTANT NOTE**
>
> In the examples used throughout the associations concepts guide, note that all references to Sails model classes are in _lowercase_.  For example, in:
```js
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
the `collection` key is set to `product`--this is the _identity_ of the Sails model called `Product`.  Whenever models are referenced in `collection`, `via`, `model` or `through` keys, their lowercased identity names should be used.

<docmeta name="displayName" value="Associations">
