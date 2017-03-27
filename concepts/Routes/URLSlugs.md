# URL Slugs
<<<<<<< HEAD
一个常见的例子就是明确的路由给设计的slugs或[vanity URLs](http://en.wikipedia.org/wiki/Clean_URL#Slug)。比如，考虑到Github上的代码库的URL，[`http://www.github.com/balderdashy/sailsjs`](http://www.github.com/balderdashy/sailsjs)。在Sails中，我们也许会定义这条路由在我们的**config/routes.js**文件的底部如下：
=======
A common use case for explicit routes is the design of slugs or [vanity URLs](http://en.wikipedia.org/wiki/Clean_URL#Slug).  For example, consider the URL of a repository on Github, [`http://www.github.com/balderdashy/sails`](http://www.github.com/balderdashy/sails).  In Sails, we might define this route at the **bottom of our `config/routes.js` file** like so:
>>>>>>> upstream/master

```javascript
'get /:account/:repo': {
controller: 'RepoController',
action: 'show',
skipAssets: true
}
```

<<<<<<< HEAD
在你的·RepoController·的·show·动作中，我们使用`req.param('account')`和`req.param('repo')`来查找合适代码库的数据，然后将它传递给[locals](http://sailsjs.org/documentation/concepts/Views/Locals.html)合适的[view](http://sailsjs.org/documentation/concepts/Views)。[`skipAssets` option](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options)选项确保无价值的的理由不会意外地匹配上我们[assets](http://sailsjs.org/documentation/concepts/Assets)中的任何一个(比如`/images/logo.png`)，所以它们仍然是可访问的。
=======
In your `RepoController`'s `show` action, we'd use `req.param('account')` and `req.param('repo')` to look up the data for the appropriate repository, then pass it in to the appropriate [view](http://sailsjs.com/documentation/concepts/Views) as [locals](http://sailsjs.com/documentation/concepts/Views/Locals.html).  The [`skipAssets` option](http://sailsjs.com/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-target-options) ensures that the vanity route doesn't accidentally match any of our [assets](http://sailsjs.com/documentation/concepts/Assets) (e.g. `/images/logo.png`), so they are still accessible.
>>>>>>> upstream/master




<docmeta name="displayName" value="URL Slugs">
