# 校验
Sails默认支持对你的模型的属性进行检验。无论什么时候一条记录被更新或者一条新的记录创建的时候，每个属性的数据都会重新被检查是否满足你的预定义的检验规则。这提供了一个便利的失效保护来确保无效的条目不会被写入到你的数据库。

<<<<<<< HEAD
除了`unique`(作为一个数据库层的约束条件；参考["Unique"](#unique))，下面所有的有效性检查都是使用JavaScript实现并且可以运行在相同的Node.js服务器进程中比如Sails。同时还需要记住无论使用什么样的有效性检测，数据类型('string'，'integer'，'json'等)这个总是需要指定。
=======
Sails bundles support for automatic validations of your models' attributes. Any time a record is updated, or a new record is created, the data for each attribute will be checked against all of your predefined validation rules. This provides a convenient failsafe to ensure that invalid entries don't make their way into your app's database(s). 

Except for `unique` (which is implemented as a database-level constraint; [see "Unique"](http://sailsjs.com/documentation/concepts/models-and-orm/validations#?unique)), all validations below are implemented in JavaScript and run in the same Node.js server process as Sails.  Also keep in mind that, no matter what validations are used, an attribute must _always_ specify one of the built in data types ('string', 'number', json', etc).
>>>>>>> upstream/master

```javascript
// User
module.exports = {
  attributes: {
    emailAddress: {
      type: 'string',
      unique: true,
      required: true
    }
  }
};
```

<<<<<<< HEAD
### 有效性规则
有效性是通过[锚点](https://github.com/sailsjs/anchor)操作，在[node-validator](https://github.com/chriso/validator.js)上层，一个Node.js使用的强健的校验库。

> 在下面这张表中， "Compatible Attribute Type(s)"列表明了每一条检测适合使用的类型(也就是对于属性定义的`type`的值)。在大部分情况下，一条检验规则可以使用多种类型。下面是一个粗略地解释：

> - If compatible with ((string)), then the validation rule is also compatible with ((text)) and ((json)).
> - If compatible with ((integer)), then the validation rule is also compatible with ((number)) and ((json)).
> - If compatible with ((float)), then the validation rule is also compatible with ((number)) and ((json)).
> - If compatible with ((number)), then the validation rule is also compatible with ((json)).
> - If compatible with ((boolean)), then the validation rule is also compatible with ((json)).
> - If compatible with ((date)), then the validation rule is also compatible with ((datetime)).
> - If compatible with ((time)), then the validation rule is also compatible with ((datetime)).
> - If compatible with ((array)), then the validation rule is also compatible with ((json)).


| Name of Rule      | What It Checks      | Notes On Usage | Compatible Attribute Type(s) |
|-------------------|---------------------|----------------|:----------------------------:|
|after| Interpret incoming string as date and ensure that it refers to a moment _after_ the configured JavaScript `Date` instance. | `after: new Date('Sat Nov 05 1605 00:00:00 GMT-0000')` | ((string)) |
|alpha| Ensure incoming string contains only uppercase and/or lowercase letters.  | (i.e. `/a-z/i`) | ((string)) |
|alphadashed|| does this `string` contain only letters and/or dashes? | ((string)) |
|alphanumeric| check if `string` in this record contains only letters and numbers. | | ((string)) |
|alphanumericdashed| does this `string` contain only numbers and/or letters and/or dashes? | | ((string)) |
|array| is this a valid javascript `array` object? | strings formatted as arrays won't pass | ((array)) |
|before| Only allow date strings that refer to a moment _before_ the configured JavaScript `Date` instance | `before: new Date('Sat Nov 05 1605 00:00:00 GMT-0000')` | ((string)) |
|binary| is this binary data? | If it's a string, it will always pass | ((string)) |
|boolean| is this a valid javascript `boolean` ? | `string`s will fail | ((boolean)) |
|contains| check if `string` in this record contains the seed | | ((string)) |
|creditcard| check if `string` in this record is a credit card | | ((string)) |
|date| check if `string` in this record is a date | takes both strings and javascript | ((string)) |
|datetime| check if `string` in this record looks like a javascript `datetime`| | ((string)) |
|decimal| | contains a decimal or is less than 1?| ((number)) |
|email| check if `string` in this record looks like an email address | | ((string)) |
|empty| Arrays, strings, or arguments objects with a length of 0 and objects with no own enumerable properties are considered "empty" | lo-dash _.isEmpty() | ((json)) |
|equals| check if `string` in this record is equal to the specified value | `===` ! They must match in both value and type | ((json)) |
|falsey| Would a Javascript engine register a value of `false` on this? | | ((json)) |
|finite| Checks if given value is, or can be coerced to, a finite number | This is not the same as native isFinite which will return true for booleans and empty strings | ((number)) or ((string)) |
|float| check if `string` in this record is of the number type float | | ((number)) or ((string)) |
|hexadecimal| check if `string` in this record is a hexadecimal number | | ((number)) or ((string)) |
|hexColor| check if `string` in this record is a hexadecimal color | | ((string)) |
|in| check if `string` in this record is in the specified array of allowed `string` values | | ((string)) |
|int| Ensure incoming value is an integer, or is a string that looks like one. | This is an alias for the `integer` rule below. | ((number)) or ((string)) |
|integer| Ensure incoming value is an integer, or is a string that looks like one. | | ((number)) or ((string)) |
|ip| check if `string` in this record is a valid IP (v4 or v6) | | ((string)) |
|ipv4| check if `string` in this record is a valid IP v4 | | ((string)) |
|ipv6| check if `string` in this record is a valid IP v6 | | ((string)) |
|is| Ensure incoming value matches the configured regular expression. | | ((string)) |
|json| does a try&catch to check for valid JSON. | | ((json)) |
|lowercase| is this string in all lowercase? | | ((string)) |
|max| | | ((number)) |
|maxLength| |  | ((string)) |
|min| | | ((number)) |
|minLength| | | ((string)) |
|not| Ensure incoming value **does not** match the configured regular expression. | | ((string)) |
|notContains| | | ((string)) |
|notEmpty| |  | ((string)) |
|notIn| Ensure incoming value **is not in** the configured array. | | ((string)) |
|notNull| Ensure incoming value **is not** equal to `null` | | ((json)) |
|null| Ensure incoming value **is `null`**. | | ((json)) |
|numeric| Ensure incoming value is a string which is parseable as a number. | Note that [while `NaN` is considered a number in JavaScript](https://www.destroyallsoftware.com/talks/wat), that is not true for the purposes of this validation. | ((string)) |
|required| Ensure incoming value is defined; that is, **not `undefined`**. | | ((json)) |
|string| Ensure incoming value is a string. | | ((string)) |
|truthy| Ensure a Javascript engine would consider the incoming value `false` if used in an `if` statement. | | ((json)) |
|undefined| Ensure incoming value is `undefined`. | | ((json)) |
|uppercase| checks if `string` in this record is uppercase | | ((string)) |
|url| Ensure incoming value is a URL. | | ((string)) |
|urlish| Ensure incoming value looks vaguely like a URL of some kind. | `/^\s([^\/]+\.)+.+\s*$/g` | ((string)) |
|uuid| checks if `string` in this record is a UUID (v3, v4, or v5) | | ((string)) |
|uuidv3| checks if `string` in this record is a UUID (v3) | | ((string)) |
|uuidv4| checks if `string` in this record is a UUID (v4) | | ((string)) |
=======
### Built-in Data Types

Every attribute definition must have a built-in data type (or _typeclass_) specified.  This is used for logical validation and coercion of results and criteria.


| Data Type        | Usage                         | Description                                                  |
|:----------------:|:----------------------------- |:------------------------------------------------------------ |
| ((string))       | `type: 'string'`              | Any string (tolerates `null`).
| ((number))       | `type: 'number'`              | Any number (tolerates `null`)
| ((boolean))      | `type: 'boolean'`             | `true` or `false` (also tolerates `null`)
| ((json))         | `type: 'json'`                | Any JSON-serializable value, including numbers, booleans, strings, arrays, dictionaries, and `null`.
| ((array))        | `type: 'array'`               | Any array consisting solelyof JSON-serializable contents.   |

Different databases vary slightly in the way they handle edge cases and special values such as `Infinity`, `null`, strings of varying lengths, etc.  Sails' ORM (Waterline) and its adapters perform loose validation to ensure that the values provided in criteria dictionaries and as values to `.create()` or `.update()` match the expected typeclass.

> Note that auto-migration also relies on the attribute's declared `type`. This is mainly relevant for schemaful databases (like MySQL or PostgreSQL), since the relevant adapter needs to use this information in order to alter/define tables during auto-migration.  Remember that in production, `migrate: 'safe'` will be enabled and auto-migration will be skipped.



### Validation Rules

The following validation rules are handled by [Anchor](https://github.com/sailsjs/anchor), a robust validation library for Node.js.

In the table below, the "Compatible Attribute Type(s)" column shows what data type(s) (i.e. for the attribute definition's `type` property) are appropriate for each validation rule.  In many cases, a validation rule can be used with more than one type.  Note that coincidentally, the table below takes a shortcut:  If compatible with ((string)), ((number)), ((boolean)), or ((array)), then the validation rule is also compatible with ((json)).


| Name of Rule      | What It Checks For                                                                                                  | Notes On Usage               | Compatible Attribute Type(s) |
|:------------------|:--------------------------------------------------------------------------------------------------------------------|:-----------------------------|:----------------------------:|
|after              | A value that, when parsed as a date, refers to moment _after_ the configured JavaScript `Date` instance.            | `after: new Date('Sat Nov 05 1605 00:00:00 GMT-0000')` | ((string)) |
|alpha              | A value that contains only uppercase and/or lowercase letters.                                                      | `alpha: true`                | ((string)) |
|alphadashed        | A value that contains only letters and dashes.                                                                      |  | ((string)) |
|alphanumeric       | A value that contains only letters and numbers.                                                                     | | ((string)) |
|alphanumericdashed | A value that is a string consisting of only letters, numbers, and/or dashes.                                        | | ((string)) |
|before             | A value that, when parsed as a date, refers to a moment _before_ the configured JavaScript `Date` instance.         | `before: new Date('Sat Nov 05 1605 00:00:00 GMT-0000')` | ((string)) |
|contains           | A value that contains the specified substring.                                                                      | `contains: 'needle'`   | ((string)) |
|creditcard         | A value that is a credit card number.                                                                               | **Do not store credit card numbers in your database unless your app is PCI compliant!**  If you want to allow users to store credit card information, a safe alternative is to use a payment API like [Stripe](https://stripe.com). | ((string)) |
|datetime           | A value that can be parsed as a timestamp; i.e. would construct a JavaScript Date with `new Date()`                 |    | ((string)) |
|_decimal_          | _Alias for `float`._ | |  |
|email              | A value that looks like an email address. | | ((string)) |
|finite             | A value that is, or can be coerced to, a finite number. | This is not the same as native isFinite which will return true for booleans and empty strings | ((number)) or ((string)) |
|float              | A value that is, or can be coerced to, a floating point (aka decimal) number. | | ((number)) or ((string)) |
|hexadecimal        | A value that is a hexadecimal number. | | ((number)) or ((string)) |
|hexColor           | A value that is a hexadecimal color. | | ((string)) |
|in                 | A value that is in the specified array of allowed strings. | | ((string)) |
|_int_              | _Alias for `integer`._       |  |  |
|integer            | A value that is an integer, or a string that looks like one. | | ((number)) or ((string)) |
|ip                 | A value that is a valid IP address (v4 or v6) | | ((string)) |
|ipv4               | A value that is a valid IP v4 address. | | ((string)) |
|ipv6               | A value that is a valid IP v6 address. | | ((string)) |
|_is_               | _Alias for `regex`._                               | |  |
|lowercase          | A value that consists only of lowercase characters. | | ((string)) |
|max                | A value that is less than the configured number. | | ((number)) |
|maxLength          | A value that has no more than the configured number of characters. |  | ((string)) |
|min                | A value that is greater than the configured number. | | ((number)) |
|minLength          | A value that has at least the configured number of characters. | | ((string)) |
|notRegex           | A value that **does not** match the configured regular expression. | | ((string)) |
|notContains        | A value that does not contain the configured substring. | e.g. `'-haystack-needle-haystack-'` would fail validation against `notContains: 'needle'` | ((string)) |
|notIn              | A value that **is not in** the configured array. | | ((string)) |
|notNull            | A value that **is not** equal to `null` | | ((json)) |
|numeric            | A value that is a string which is parseable as a number. | Note that [while `NaN` is considered a number in JavaScript](https://www.destroyallsoftware.com/talks/wat), that is not true for the purposes of this validation. | ((string)) |
|required           | A value that is defined; that is, **not `undefined`**. | | ((json)) |
|regex              | A value that matches the configured regular expression. | | ((string)) |
|truthy             | A value that would be considered truthy if used in a JavaScript `if` statement. | | ((json)) |
|uppercase          | A value that is uppercase. | | ((string)) |
|url                | A value that is a URL. | | ((string)) |
|urlish             | A value that looks vaguely like a URL of some kind (i.e. `/^\s([^\/]+\.)+.+\s*$/g`). | `urlish: true` | ((string)) |
|uuid               | A value that is a UUID (v3, v4, or v5) | | ((string)) |
|uuidv3             | A value that is a UUID (v3) | | ((string)) |
|uuidv4             | A value that is a UUID (v4) | | ((string)) |
>>>>>>> upstream/master


### 唯一性
`unique`不同于上面罗列的所有的检验规则。实际上，它不完全算是一条检验规则：它是**数据库级别的约束条件**。

如果一个属性声明自己为`unique: true`，那么Sails要确保不会出现有两条记录有相同的值。最常见的例子便是在一个`User`模型中的`emailAddress`属性：

```javascript
// api/models/User.js
module.exports = {

  attributes: {
    emailAddress: {
      type: 'string',
      unique: true,
      required: true
    }
  }

};
```

##### 为什么唯一性有别于其他检验？
想象在你的数据库中有1000000个用户记录。如果`unique`实现得和其他校验一样，方每次有新用户注册进来的时候，Sails将会需要去查找将近已经存在的*百万条*记录以确保没人使用新用户提供的邮件地址。不仅这样会很慢，而且当我们搜索完所有的记录的时候，别人可能已经注册了。

<<<<<<< HEAD
幸运的是，这种类型的唯一性校验可能是*任何*数据库最普遍的一个特性。为了利用这个，Sails依赖于[database adapter](http://sailsjs.org/documentation/concepts/models-and-orm#?adapters)来实现对`unique`的支持--特别地，通过在数据库自己 [auto-migration](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings#?migrate)的时候添加一个**唯一性约束条件**到相关的字段/列/属性中。也就是当你的app被设置成`migrate:'alter'`的时候，Sails将会自动地在底层数据库中生成表/集合，并且内建了唯一性的约束条件。一旦你切换到`migrate:'safe'`，更新数据库就完全取决于你了。
=======
Fortunately, this type of uniqueness check is perhaps the most universal feature of _any_ database.  To take advantage of that, Sails relies on the [database adapter](http://sailsjs.com/documentation/concepts/models-and-orm#?adapters) to implement support for the `unique` validation-- specifically, by adding a **uniqueness constraint** to the relevant field/column/attribute in the database itself during [auto-migration](http://sailsjs.com/documentation/concepts/models-and-orm/model-settings#?migrate).  That is, while your app is set to `migrate:'alter'`, Sails will automatically generate tables/collections in the underlying database with uniqueness constraints built right in.  Once you switch to `migrate:'safe'`, updating your database constraints is up to you.
>>>>>>> upstream/master

##### 索引呢？
当你开始使用你的产品数据库的时候，通过设置索引来提高你的数据库性能总是是一个好主意。在数据库中设置索引的最佳实践超出了本文档的范围。即便如此如果你之前没有这么做，那么不用担心--[因为事实上这个比你想的容易得多](http://stackoverflow.com/a/1130/486547)。

只需要像你的产品架构的其他部分一样，银弹你设置你的app去使用`migrate: 'safe'`，Sails是否留下数据库的索引完全取决于你。

> 注意这意味着你应该确保更新你的索引和你的唯一性约束条件当执行[manual migrations](https://github.com/BlueHotDog/sails-migrations)的时候。

### 什么时候使用检验
校验可以说是一个巨大的时间节省器，防止你写数百行重复的代码。但是值得注意的是模型校验是为了*每一次的创建或更新*运行的。在你的其中一条属性定义使用检验规则之前，确保你*每次*在调用`.create()`或`.update()`去指定一个新的值到那个属性的时候你都能应用到那校验规则。如果事实并非如此，那么写代码去校验在你的控制器中输入的值；或者在你的[services](http://sailsjs.org/documentation/concepts/services)或一个[model class method](http://sailsjs.org/documentation/concepts/models-and-orm/models#?model-methods-aka-static-or-class-methods)调用一个自定义的函数。

<<<<<<< HEAD
比如，假设你的Sails app允许用户通过输入邮箱地址和密码注册一个账号然后确认邮件地址或者通过Linkedln注册。现在你的`User`模型有一个叫做`linkedInEmail`的属性和另外一个叫做`manuallyEnteredEmail`的属性。虽然那些邮箱地址属性中的某一个是要求填写的，但是*哪一个要求*的确实取决于一个用户是怎么注册的。所以在这种情况下，你的`User`模型不能使用`required: true`校验--而是应该检验其中的一个邮箱是否提供了并且在调用`.create()`和 `.update()`之前手工检查这些值是否有效：比如
=======

### When to Use Validations

Validations can be a huge time-saver, preventing you from writing many hundreds of lines of repetitive code.  But keep in mind that model validations are run for _every create or update_ in your application.  Before using a validation rule in one of your attribute definitions, make sure you are OK with it being applied _every time_ your application calls `.create()` or `.update()` to specify a new value for that attribute.  If that is _not_ the case, write code that validates the incoming values inline in your controller; or call out to a custom function in one of your [services](http://sailsjs.com/documentation/concepts/services), or a [model class method](http://sailsjs.com/documentation/concepts/models-and-orm/models#?model-methods-aka-static-or-class-methods).

For example, let's say that your Sails app allows users to sign up for an account by either (A) entering an email address and password and then confirming that email address or (B) signing up with LinkedIn.  Now let's say your `User` model has one attribute called `linkedInEmail` and another attribute called `manuallyEnteredEmail`.  Even though _one_ of those email address attributes is required, _which one_ is required depends on how a user signed up.  So in that case, your `User` model cannot use the `required: true` validation-- instead you'll need to validate that one email or the other was provided and is valid by manually checking these values before the relevant `.create()` and `.update()` calls in your code, e.g.:
>>>>>>> upstream/master

```javascript
if ( !_.isString( req.param('email') ) ) {
  return res.badRequest();
}
```


为了使用这一步，我们假设你的应用接受付款。在注册期间，如一个用户使用一个支付计划来注册，那么他或她必须也提供一个邮箱地址来作为账单地址(`billingEmail`)。如果一个用户使用免费账户来注册，那么他或她将会忽略这个步骤。在用户设置页面上，使用支付计划的用户将会看到一个“Billing Email”的表单字段。这样他们可以自定义自己的账单地址。这个有别于那些免费计划的账户，但是这些用户反而会有一个“Upgrade Plan”页面来调用到升级的动作。

虽然有这些看起来很专用的需求，但是仍然有一些还没有回答的问题：
+ 当其他的邮箱地址从默认的配置改变来的时候我们是否应该自动地更新账单地址？
+ 如果账单地址不止一次地改变又该怎样？
+ 当一个用户降级到免费计划的时候账单地址又该怎样？如果一个用户再次升级到一个支付的计划，那么我们是该使用它之前使用的旧的账单地址还是新的？
+ 当一个已经存在的用户连接到它的Linkedln账户的时候并且一个新的`linkedInEmail`保存的时候账单地址又该如何？
+ 如果每个月的发票邮件无法送达的时候账单地址又该如何？
+ 如果你支持的小组的某个成员通过管理员接口登录进去并且手动地修改那么账单地址又该如何？
+ 如果在我们提供给Linkdln API的回调URL中收到一个POST请求来通知我们的app说用户改变了在[ http://linkedin.com]( http://linkedin.com)的邮箱地址并且保存了一个新的`linkedInEmail`的时候账单地址又该如何？
+ 当一个已经存在的yoghurt断开它的Linkedln账户那么账单地址又该如何？
+ 在数据库中的连个账号是否允许有相同的账单地址？那么Linkedln的邮箱地址呢？或者他们它们手工地输入其中的某个？

基于这些问题的答案，我们可能会将`required`的校验放在`billingEmail`上，并添加新的属性(比如`hasBillingEmailBeenChangedManually`)，或者甚至考虑是否使用一个`unique`的约束条件。

<<<<<<< HEAD
最后，还有下面一些小提示：
+ 关于是否为你的一个特别的属性使用校验的最初决定取决于你的app的需求以及你是如何调用`.update()`和`.create()`。不要害怕忘记内建的检验支持以及在你的控制器中手动检查值。这个总是最干净最可维护的方法。
+ 因为你的app的演进导致从你的模型中添加或者删除检测是很正常的事情。但是一旦你进入产品模式，那么**有一个很重要的例外**：`unique`。在开发阶段，当你的app配置成使用[`migrate: 'alter'`](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings#?migrate)，那么你可以随意添加或者删除`unique`的检测。但是如果你使用了`migrate: safe`(比如用在你的产品数据库上)，那么你想要在你的数据库中更新约束条件或者索引，以及[migrate your data by hand](https://github.com/BlueHotDog/sails-migrations)。
+ 在花费大量的时间来设置你的模型属性的复杂的校验之前*首先*花费时间来完全地理解你的应用的用户接口是很明智的。

> 在你花费大量的时间去实现后端通用代码之前你应该尽可能 的获得或者具体化你的app的用户接口的线框。当然，这并不总是可能的--[blueprints](http://sailsjs.org/documentation/concepts/blueprints)便是为了这个。但是你应该意识到内建了一个UI中心或“前端优先”哲学的应用总是很容易维护，也很少有bug，因为他们重建了完整的用户接口只是并且有更多的优雅的APIs。
=======
### Best Practices

Finally, here are a few tips:
- Your initial decision about whether or not to use validations for a particular attribute should depend on your app's requirements and how you are calling `.update()` and `.create()`. Don't be afraid to forgo built-in validation support and check values by hand in your controllers or in a helper function.  Oftentimes this is the cleanest and most maintainable approach.
- There's nothing wrong with adding or removing validations from your models as your app evolves. But once you go to production, there is one **very important exception**: `unique`.  During development, when your app is configured to use [`migrate: 'alter'`](http://sailsjs.com/documentation/concepts/models-and-orm/model-settings#?migrate), you can add or remove `unique` validations at will.  However, if you are using `migrate: safe` (e.g. with your production database), you will want to update constraints/indices in your database, as well as [migrate your data by hand](https://github.com/BlueHotDog/sails-migrations).
- It is a very good idea to spend the time to fully understand your application's user interface _first_ before setting up complex validations on your model attributes.

> As much as possible, it is a good idea to obtain or flesh out your own wireframes of your app's user interface _before_ you spend any serious amount of time implementing _any_ backend code.  Of course, this isn't always possible- and that's what the [blueprint API](http://sailsjs.com/documentation/concepts/blueprints) is for.  Applications built with a UI-centric, or "front-end first" philosophy are easier to maintain, tend to have fewer bugs and, since they are built with full knowledge of the user interface from the get-go, they often have more elegant APIs.
>>>>>>> upstream/master


### 自定义有效性规则
> **警告**：自定义校验规则的文档支持很可能在Waterline1.0的时候停止。为了你的app不会过时，请从你的[services](http://sailsjs.org/documentation/concepts/services)或一个[model class method](http://sailsjs.org/documentation/concepts/models-and-orm/models#?model-methods-aka-static-or-class-methods)使用一个函数来自定义检测。


你可以定义你自己的自定义检测通过指定一个`type`的目录作为你的模型的属性的顶层，然后在你的属性定义中使用它们就像上面其他检测的使用一样：


<<<<<<< HEAD
=======
> **Warning:** Support for custom validation rules as documented here will very likely be ending in Waterline 1.0.  To future-proof your app, use a function from one of your [services](http://sailsjs.com/documentation/concepts/services) or a [model class method](http://sailsjs.com/documentation/concepts/models-and-orm/models#?model-methods-aka-static-or-class-methods) for custom validation instead.
>>>>>>> upstream/master


```javascript
// api/models/User.js
module.exports = {

  // Values passed for creates or updates of the User model must obey the following rules:
  attributes: {

    firstName: {
      // Note that a base type (in this case "string") still has to be defined, even though validation rules are in use.
      type: 'string',
      required: true,
      minLength: 5,
      maxLength: 15
    },

    location: {
      type: 'json',
      isPoint: true // << defined below
    },

    password: {
      type: 'string',
      password: true // << defined below
    }

  },

  // Custom types / validation rules
  // (available for use in this model's attribute definitions above)
  types: {
    isPoint: function(value){
      // For all creates/updates of `User` records that specify a value for an attribute
      // which declares itself `isPoint: true`, that value must:
      // • be a dictionary with numeric `x` and `y` properties
      // • both `x` and `y` must be neither `Infinity` nor `-Infinity`
      return _.isObject(value) &&
      _.isNumber(value.x) && _.isNumber(value.y) &&
      value.x !== Infinity && value.x !== -Infinity &&
      value.y !== Infinity && value.y !== -Infinity;
    },
    password: function(value) {
      // For all creates/updates of `User` records that specify a value for an attribute
      // which declares itself `type: 'password'`, that value must:
      // • be a string
      // • be at least 6 characters long
      // • contain at least one number
      // • contain at least one letter
      return _.isString(value) && value.length >= 6 && value.match(/[a-z]/i) && value.match(/[0-9]/);
    }
  }
}
```

自定义检测函数在接收作为它们的第一个参数的被检测的入口值，并且如果有效的话返回`true`，否则返回`false`。一旦设置完成，这些自定义的检测规则可以用在模型中的多个属性上，通过设置一个与相关属性定义的相同名字的额外属性上(比如`someCustomValidationRuleOrType: true`)。

注意自定义检测规则在内建的校验和类型中不是namespaced的--它们全被合并在一起了。所以小心不要去定义一个自定义的检测与其他的基本类型或者在Waterline中的检测冲突(比如不要命名你的自定义检测规则为`string`或`minLength`)。



##### 自定义有效性消息
Sails.js不支持自定义有效性消息。所以在你的`create()`或`update()`调用中的回调中你的代码应该查找校验错误并采取合适的措施。不管怎样都应该在你的JSON响应中发送一个特定的错误代码或者渲染合适的消息到一个HTML错误页面上

<<<<<<< HEAD
> 如果你在使用Sails v0.11.0+，你也许想要利用[`sails-hook-validation`](https://github.com/lykmapipo/sails-hook-validation)，一个由[@lykmapipo](http://github.com/lykmapipo)制作的[custom hook](http://sailsjs.org/documentation/concepts/extending-sails/Hooks)。它的安装和使用可以在[`sails-hook-validation` repository on GitHub](https://github.com/lykmapipo/sails-hook-validation)中找到。
=======
> If you are using Sails v0.11.0+, you may want to take advantage of [`sails-hook-validation`](https://github.com/lykmapipo/sails-hook-validation), a [custom hook](http://sailsjs.com/documentation/concepts/extending-sails/Hooks) by [@lykmapipo](http://github.com/lykmapipo).  Details regarding its installation and usage can be found in the [`sails-hook-validation` repository on GitHub](https://github.com/lykmapipo/sails-hook-validation).
>>>>>>> upstream/master



<docmeta name="displayName" value="Validations">
