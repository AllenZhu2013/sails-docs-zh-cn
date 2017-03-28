### 动态翻译内容
如果你的后端存储多国语言的数据(比如通过CMS产品数据被录成多国语言)，你不应该回应一个简单的JSON本地化文件除非你计划动态地编辑本地化。有一个选项是用于编辑可编程的本地化翻译，可以通过一个自定义的实现也可以通过一个翻译服务。Sails/node-i18n的JSON字符串文件兼容[webtrslateit.com](https://webtranslateit.com/en)使用的格式。

在另一方面你可能需要存储这些动态翻译的字符串类型到数据库中。如果是这样的话，只需要确保并编译你相应的数据模型，这样你就可以通过locale id存储和检索动态数据(比如"en"、"es"、"de"等)。那样的话，你就可以利用[`req.getLocale()`](https://github.com/mashpie/i18n-node#getlocale)方法帮助你解决使用在任何响应中的翻译的内容，并且保持在你的app中使用的会话一致性。
<docmeta name="displayName" value="Translating Dynamic Content">
