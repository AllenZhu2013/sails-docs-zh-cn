# Associations
使用Sails和Waterline的时候，你可以关联模型到多个数据存储中。这意味着即使你的users存储在[PostgreSQL](http://www.postgresql.org/)和你的照片放在[MongoDb](http://www.mongodb.com/)，你也可以交互这些数据就好像它们是在同一个数据库一样。你也可以使用相同的适配器关联到不同范围的[连接](http://sailsjs.org/documentation/reference/sails.config/sails.config.connections.html)(也就是数据存储或数据库)。这样很方便如果比如你的app需要访问或者更新传统的存储在一个[MySQL](http://www.mysql.com/)中的食谱数据在你的公司的数据中心，而且还可以从一个云上的一个新的MySQL数据中存储和获取配料数据。


<docmeta name="displayName" value="Associations">
