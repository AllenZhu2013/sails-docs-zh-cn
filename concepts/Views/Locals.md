# 本地变量
在一个特殊的视图中可访问到的变量我们叫做`locals`。本地变量表示的是你的视图可以访问到的服务器端的数据--本地变量没有实际*包含*到你的编译的HTML除非你明确地使用你的视图引擎提供的特殊语法来引用它们。

```ejs
<div>Logged in as <a><%= name %></a>.</div>
```

##### 在你的视图中使用本地变量
下面提到的符号是用于访问你的本地变量，在EJS中，你使用特殊的模板标记(比如`<%= someValue %>`)来包含本地变量到你的视图中。

在EJS中有下面三种类型的模板标签：
+ `<%= someValue %>`
    + HTML-转义`someValue`这个本地变量，然后将其作为一个字符串来包含；
+ `<%- someRawHTML %>`
    + 一字不差地包含`someRawHTML`这个本地变量，不进行转义；
    + 请注意，这个标签会让你容易受到XSS攻击如果你不知道你在做什么。
+ `<% if (!loggedIn) { %> <a>Logout</a> <% } %>`
    + 当视图编译的时候执行`<% ... %>`里面的JS；
    + 条件语句使用(`if/else`)，循环数据使用(`for/each`)；

下面文件(`views/backOffice/profile.ejs`)的视图例子使用了两个本地变量：`user`和`corndogs`：

```html
<div>
  <h1><%= user.name %>'s first view</h1>
  <h2>My corndog collection:</h2>
  <ul>
    <% _.each(corndogs, function (corndog) { %>
    <li><%= corndog.name %></li>
    <% }) %>
  </ul>
</div>
```

> 你也许可能发现另外一个本地变量-`_`。默认的，Sails会自动传递几个本地变量到你的视图中，这些变量都包含lodash(`_`)。

如果你想传递给视图文件的数据是完全静态的，你不再需要一个控制器-你只需要固定你的视图文件，然后本地变量都放在你的`config/routes.js`文件中，如下：

```javascript
  // ...
  'get /profile': {
    view: 'backOffice/profile',
    locals: {
      user: {
        name: 'Frank',
        emailAddress: 'frank@enfurter.com'
      },
      corndogs: [
        { name: 'beef corndog' },
        { name: 'chicken corndog' },
        { name: 'soy corndog' }
      ]
    }
  },
  // ...
```


另一方面，在大部分场景下这些数据是动态的，我们可能需要控制器的动作来从我们的模型中加载，然后通过res.view()方法传递给视图。

假设现在我们将我们的路由关联到控制器中的动作(并且已经设置了模型)，我们可能应该像下面这样的设置：

```javascript
// in api/controllers/UserController.js...

  profile: function (req, res) {
    // ...
    return res.view('backOffice/profile', {
      user: theUser,
      corndogs: theUser.corndogCollection
    });
  },
  // ...
```


<docmeta name="displayName" value="Locals">
