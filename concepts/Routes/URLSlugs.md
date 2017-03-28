# URL Slugs
一个常见的例子就是明确的路由给设计的slugs或[vanity URLs](http://en.wikipedia.org/wiki/Clean_URL#Slug)。比如，考虑到Github上的代码库的URL，[`http://www.github.com/balderdashy/sailsjs`](http://www.github.com/balderdashy/sailsjs)。在Sails中，我们也许会定义这条路由在我们的**config/routes.js**文件的底部如下：

```javascript
'get /:account/:repo': {
controller: 'RepoController',
action: 'show',
skipAssets: true
}
```

在你的·RepoController·的·show·动作中，我们使用`req.param('account')`和`req.param('repo')`来查找合适代码库的数据，然后将它传递给[locals](http://sailsjs.org/documentation/concepts/Views/Locals.html)合适的[view](http://sailsjs.org/documentation/concepts/Views)。[`skipAssets` option](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options)选项确保无价值的的理由不会意外地匹配上我们[assets](http://sailsjs.org/documentation/concepts/Assets)中的任何一个(比如`/images/logo.png`)，所以它们仍然是可访问的。




<docmeta name="displayName" value="URL Slugs">
