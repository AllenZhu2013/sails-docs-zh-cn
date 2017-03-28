# Partials(局部模板)
在它的视图渲染代码中Sails使用`ejs-locals`，所以在你的视图中你可以这样做：


```
<%- partial ('foo.ejs') %>
```

来渲染位于`/views/foo.ejs`中的一个局部模板。你的所有的本地变量将会自动地被发送到局部模板中。

加载局部模板的路径都是相对于视图来说的。所以如果你在`/views/users/view.ejs`中有一个用户视图并且想要加载`/views/partials/widget.ejs`，那么你应该这么写：

```
<%- partial ('../partials/widget.ejs') %>
```

有一件事情需要注意：局部模板是同步渲染的，所以他们会在Sails服务于多个请求的时候锁住知道他们都加载完成。有一些需要记住的是当开发你的app时，尤其如果你预期你的app会有很多的连接的时候。

> 注意：

> 当你使用其他模板引擎而不是EJS的时候，它们加载局部模板的语法或者被锁的机制都会被使用。请参考关于其他模板引擎的文档。

<docmeta name="displayName" value="Partials">

