# Waterline查询语言
Waterline查询语言是一个基于对象标准用来从支持的数据库适配器中获取数据。这意味着你可以使用MySQL和Redis、MongoDb一样的查询。这允许你改变你的数据库而不需要改变代码。

<<<<<<< HEAD
> 在Waterline内部的所有查询都是区分大小写的。这让查询变得更加容易但是也让字符串索引变得困难。如果你正在索引并且搜寻字符串字段的时候这个地方值得让你注意一下。

### 查询语言基础
标准对象是使用4个对象关键词中的一个来组成。这些都是顶层关键词用在一个查询对象中的。用在MongoDb上的是宽松的标准，所以有一些细微的差别。

查询既可以使用允许使用查询选项比如`limit`和`skip`的`where`关键词来指定属性，也可以在如果`where`不包含在整个对象之内的情况被作为一个`where`的标准。
=======
The Waterline Query language is an object-based syntax used to retrieve the records from any supported database.  Under the covers, Waterline uses the database adapter(s) installed in your project to translates this language into native queries, and then to send those queries to the appropriate database.  This means that you can use the same query with MySQL as you do with Redis, or MongoDb. And it allows you to change your database with minimal (if any) changes to your application code.

> All queries inside of Waterline are case-insensitive. While this allows for more consistent querying across databases, depending on the database you're using, it can make indexing strings tough.  This is something to be aware of if you plan to create indexes in your database to optimize the performance of searching on string fields.

### Query Language Basics

The criteria objects are formed using one of four types of object keys. These are the top level
keys used in a query object. It is loosely based on the criteria used in MongoDB with a few slight variations.

Queries can be built using either a `where` key to specify attributes, which will allow you to also use query options such as `limit` and `skip` or, if `where` is excluded, the entire object will be treated as a `where` criteria.
>>>>>>> upstream/master

```javascript

Model.find({
  name: 'mary'
}).exec(function (err, peopleNamedMary){

});


// OR


Model.find({
  where: { name: 'mary' },
  skip: 20,
  limit: 10,
  sort: 'createdAt DESC'
}).exec(function(err, thirdPageOfRecentPeopleNamedMary){

});
```

#### Key Pairs
一个密钥对可以用于搜索匹配精确指定的值的记录。这是对象标准的一个基础，密钥在一个模型中代表一个属性，它的值是严格等于记录匹配上的值。

```javascript
Model.find({
  name: 'lyra'
}).exec(function (err, peopleNamedLyra) {

});
```

它们也可以一起使用以搜索多条记录：

```javascript
Model.find({
  name: 'walter',
  state: 'new mexico'
}).exec(function (err, waltersFromNewMexico) {
  
});
```

#### Modified Pairs
修改对也有关键词的模型属性但是它们也使用任何一个支持的标准修改器来执行查询，严格相等检查将没有用。

```javascript
Model.find({
  name : {
    'contains' : 'yra'
  }
})

#### In Pairs
<<<<<<< HEAD
IN查询的工作原理类似于MySQL的“in queries”。每一个在数组中的元素都被当做“or“：
=======

Provide an array to find records whose value for this attribute exactly matches (case-insensitive) _any_ of the specified search terms.

> This is more or less equivalent to "IN" queries in SQL, and the `$in` operator in MongoDB.
>>>>>>> upstream/master

```javascript
Model.find({
  name : ['walter', 'skyler']
}).exec(function (err, waltersAndSkylers){

});
```

#### Not-In Pairs
<<<<<<< HEAD
Not-In查询的工作原理类似于`in`查询。除了那嵌套对象标准：
=======

Provide an array wrapped in a dictionary under a `!` key (like `{ '!': [...] }`) to find records whose value for this attribute _ARE NOT_ exact matches (case-insensitive) for any of the specified search terms.

> This is more or less equivalent to "NOT IN" queries in SQL, and the `$nin` operator in MongoDB.
>>>>>>> upstream/master

```javascript
Model.find({
  name: { '!' : ['walter', 'skyler'] }
}).exec(function (err, everyoneExceptWaltersAndSkylers){

});
```


<<<<<<< HEAD
#### Or Pairs
使用一个查询数组对来执行`OR`查询，任何匹配上再数组里面的标准对象都会被返回：
=======
Use the `or` modifier to match _any_ of the nested rulesets you specify as an array of query pairs.  For records to match an `or` query, they must match at least one of the specified query pairs in the `or` array.
>>>>>>> upstream/master

```javascript
Model.find({
  or : [
    { name: 'walter' },
    { occupation: 'teacher' }
  ]
}).exec(function(err, waltersAndTeachers){

});
```


### 标准的修改器
下面的修改器当构建查询的时候可以用到。

* `'<'` / `'lessThan'`
* `'<='` / `'lessThanOrEqual'`
* `'>'` / `'greaterThan'`
* `'>='` / `'greaterThanOrEqual'`
* `'!'` / `'not'`
* `'like'`
* `'contains'`
* `'startsWith'`
* `'endsWith'`


####  '<' / 'lessThan'

查找小于指定值的记录。

```javascript
Model.find({ age: { '<': 30 }})
```

####  '<=' / 'lessThanOrEqual'

查找小于或等于指定值的记录。

```javascript
Model.find({ age: { '<=': 21 }})
```

####  '>' / 'greaterThan'

查找大于指定值的记录。

```javascript
Model.find({ age: { '>': 18 }})
```

####  '>=' / 'greaterThanOrEqual'

查找大于或等于指定值的记录。

```javascript
Model.find({ age: { '>=': 21 }})
```

####  '!' / 'not'

查找与指定值不相等的记录。

```javascript
Model.find({
  name: { '!': 'foo' }
})
```

<<<<<<< HEAD
####  'like'

使用类型匹配`%`符号(大小写区分)查找记录。
=======
#### 'contains'

Searches for records where the value for this attribute _contains_ the given string. (Case insensitive.)
>>>>>>> upstream/master

```javascript
Model.find({
  subject: { contains: 'music' }
}).exec(function (err, musicCourses){
  
});
```

<<<<<<< HEAD
####  'contains'

字符串两端的类型匹配的缩写形式。这个查找将会返回包含该字符串的值。(区分大小写)
=======
#### 'startsWith'

Searches for records where the value for this attribute _starts with_ the given string. (Case insensitive.)
>>>>>>> upstream/master

```javascript
Model.find({
  subject: { startsWith: 'american' }
}).exec(function (err, coursesAboutAmerica){

});
```

<<<<<<< HEAD
####  'startsWith'

字符串右边的类型匹配的缩写形式。这个查找将会返回右边是以该字符串开始的值。(区分大小写)
=======
#### 'endsWith'

Searches for records where the value for this attribute _ends with_ the given string. (Case insensitive.)
>>>>>>> upstream/master

```javascript
Model.find({
  subject: { endsWith: 'history' }
}).exec(function (err, historyCourses) {

})
```

<<<<<<< HEAD
####  'endsWith'

字符串左边边的类型匹配的缩写形式。这个查找将会返回右边是以该字符串结束的值。(区分大小写)
=======
#### 'like'

Searches for records using pattern matching with the `%` sign. (Case insensitive.)
>>>>>>> upstream/master

```javascript
Model.find({ food: { 'like': '%beans' }})
```

####  'Date Ranges'

通过使用操作符的比较来做时间查询结果排序。

```javascript
Model.find({ date: { '>': new Date('2/4/2014'), '<': new Date('2/7/2014') } })

### 查询选项

查询选项允许你重定义一个查询返回的记过。目前可用的选项有：

* `limit`
* `skip`
* `sort`

####  Limit

限制查询返回的结果的个数。

```javascript
Model.find({ where: { name: 'foo' }, limit: 20 })
```

#### Skip

返回所有的结果除了那些忽略的条目数

```javascript
Model.find({ where: { name: 'foo' }, skip: 10 });
```

##### Pagination

`skip`和`limit`可以一起用来构建一个页码系统。

```javascript
Model.find({ where: { name: 'foo' }, limit: 10, skip: 10 });
```

`paginate`是一个Waterline帮助器方法，可以完成和`skip`和`limit`一样的功能。

``` javascript
Model.find().paginate({page: 2, limit: 10});
```

> **Waterline**
>
<<<<<<< HEAD
> 你可以在下面的文档中找到更多的Waterline API
> * [Sails.js Documentation](http://sailsjs.org/documentation/reference/waterline/queries)
=======
> You can find out more about the Waterline API below:
> * [Sails.js Documentation](http://sailsjs.com/documentation/reference/waterline/queries)
>>>>>>> upstream/master
> * [Waterline README](https://github.com/balderdashy/waterline/blob/master/README.md)
> * [Waterline Documentation](https://github.com/balderdashy/waterline-docs)
> * [Waterline Github Repository](https://github.com/balderdashy/waterline)

#### Sort

返回的结果可以根据属性名称进行排序。简单地指定一个属性名称作原始的排序(升序)，或者指定一个`asc`或`desc`标志位分别进行升序或降序排列。

```javascript
// Sort by name in ascending order
Model.find({ where: { name: 'foo' }, sort: 'name' });

// Sort by name in descending order
Model.find({ where: { name: 'foo' }, sort: 'name DESC' });

// Sort by name in ascending order
Model.find({ where: { name: 'foo' }, sort: 'name ASC' });

// Sort by binary notation
Model.find({ where: { name: 'foo' }, sort: { 'name': 1 }});

// Sort by multiple attributes
Model.find({ where: { name: 'foo' }, sort: { name:  1, age: 0 });
```

> **Case-sensitivity**
>
> 在Waterline内部的所有查询都是区分大小写的。这让查询变得更加容易但是也让字符串索引变得困难。如果你正在索引并且搜寻字符串字段的时候这个地方值得让你注意一下。
>
<<<<<<< HEAD
> 当前，执行**case-sensitive**查询的最好方法是[`.native()`](http://sailsjs.org/documentation/reference/waterline/models/native.html)或[`.query()`](http://sailsjs.org/documentation/reference/waterline/models/query.html)方法。
=======
> Currently, the best way to execute **case-sensitive** queries is using the [`.native()`](http://sailsjs.com/documentation/reference/waterline/models/native.html) or [`.query()`](http://sailsjs.com/documentation/reference/waterline/models/query.html) method.

>>>>>>> upstream/master

<docmeta name="displayName" value="Query Language">
