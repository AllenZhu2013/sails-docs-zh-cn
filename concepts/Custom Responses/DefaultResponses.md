# 默认响应
在`/api/responses`文件夹下面所有的Sails app都有绑定下面这些默认的响应函数。每个响应都会发送一个标准的JSON对象如果客户端需要接收JSON的话，该对象包含一个表明HTTP状态的`status`关键词和其他关于任何错误的相关信息的关键词。

#### res.ok()

这个方法用于发送一个200("Ok")的响应返回给客户端，以告知请求的内容工作正常。 👍 使用方法参考[`res.ok()` reference page](http://sailsjs.org/documentation/reference/response-res/res-ok)。

#### res.serverError()
这个方法用于发送一个500 ("Server Error")的响应返回给客户端，以告知服务器发生某种错误。 💣 使用方法参考[`res.serverError()` reference page](http://sailsjs.org/documentation/reference/response-res/res-server-error)。

#### res.badRequest()
这个方法用于发送一个400 ("Bad Request")的响应返回给客户端，以告知请求是无效的， 👎 使用方法参考[`res.badRequest()` reference page](http://sailsjs.org/documentation/reference/response-res/res-bad-request)。

#### res.notFound()
这个方法用于发送一个404 ("Not Found")的响应返回给客户端，以告知请求的URL没有在服务器中找到任何匹配的路由。 💔 使用方法参考[`res.notFound()` reference page](http://sailsjs.org/documentation/reference/response-res/res-not-found)。

#### res.forbidden(message)
这个方法用于发送一个403 ("Forbidden")的响应返回给客户端，以告知请求不被允许。 🚫 使用方法参考[`res.forbidden()` reference page](http://sailsjs.org/documentation/reference/response-res/res-forbidden)。

<docmeta name="displayName" value="Default Responses">
