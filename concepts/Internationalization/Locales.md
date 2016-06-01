# 区域设置
### 概述
i18n钩子从你的工程中“locales”目录(默认是`config/locales`)读取JSON格式的翻译文件。每一个文件对应一个你的Sails后端支持的[locale](http://en.wikipedia.org/wiki/Locale)(一般是一种语言)。

这些文件包含本地专用的字符串(是JSON键值对)，这样你就可以在你的视图和控制器中使用它们了。

下面是locale文件的实例(`config/locales/es.json`)：

```json
{
    "Hello!": "Hola!",
    "Hello %s, how are you today?": "¿Hola %s, como estas?"
}
```
注意在你的字符串文件中的关键词是**区分大小写**的并且要求精确匹配。在最好的办法中存在各种流派的不同，它确实是依赖于你将会如何编辑你的字符串文件和HTML文件。尤其如果你手动编辑翻译文件，为了简单和可维护性，会优先考虑所有的关键词名字都是小写。

比如，这是`config/locales/es.json`文件另外一种版本：

```json
{
    "hello": "Hola!",
    "hello-how-are-you-today": "Hola %s, ¿cómo estás?"
}
```

`config/locales/en.json`：

```json
{
    "hello": "Hello!",
    "hello-how-are-you-today": "Hello, how are you today?"
}
```

你也可以嵌套本地化字符串。不过最好的办法是使用`.`来代表嵌套的字符串。比如，这是一个用户控制器的索引页面做的标签列表：

``` json
{
    "user.index.label.id": "User ID",
    "user.index.label.name": "User Name"
}
```

### 检测并/或重写一个请求需要的本地化
为了确定当前请求需要的本地化，使用[`req.getLocale()`](https://github.com/mashpie/i18n-node#getlocale)。

为了重写一个请求中自动检测的语言或者本地化，使用[`req.setLocale()`](https://github.com/mashpie/i18n-node#setlocale)，调用该函数并带入新的本地化唯一的代码：

```js
// Force the language to German for the remainder of the request:
req.setLocale('de');
// (this will use the strings located in `config/locales/de.json` for translation)
```

默认node-i18n回去检测一个请求要求的语言通过检测它的语言头部。语言的头部是在你的浏览器的设置中被设置的，因为他们大部分时间都是对的。所以你可能需要灵活地重写这个检测到的本地化并提供你自己的本地化。

比如，如果你的app允许用户去挑选一个自己喜欢的语言，你也许需要创建一个[policy](http://sailsjs.org/documentation/concepts/Policies)去检查在客户的session中定制的语言，并且如果有一个存在的话你就应该在随后的策略、控制器的动作以及视图中设置合适的本地化。

```js
// api/policies/localize.js
module.exports = function(req, res, next) {
  req.setLocale(req.session.languagePreference);
  next();
};
```

<docmeta name="displayName" value="Locales">
