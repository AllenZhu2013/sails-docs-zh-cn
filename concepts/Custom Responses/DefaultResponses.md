# 默认响应
在`/api/responses`文件夹下面所有的Sails app都有绑定下面这些默认的响应函数。每个响应都会发送一个标准的JSON对象如果客户端需要接收JSON的话，该对象包含一个表明HTTP状态的`status`关键词和其他关于任何错误的相关信息的关键词。

#### res.ok()

<<<<<<< HEAD
这个方法用于发送一个200("Ok")的响应返回给客户端，以告知请求的内容工作正常。 👍 使用方法参考[`res.ok()` reference page](http://sailsjs.org/documentation/reference/response-res/res-ok)。

#### res.serverError()
这个方法用于发送一个500 ("Server Error")的响应返回给客户端，以告知服务器发生某种错误。 💣 使用方法参考[`res.serverError()` reference page](http://sailsjs.org/documentation/reference/response-res/res-server-error)。

#### res.badRequest()
这个方法用于发送一个400 ("Bad Request")的响应返回给客户端，以告知请求是无效的， 👎 使用方法参考[`res.badRequest()` reference page](http://sailsjs.org/documentation/reference/response-res/res-bad-request)。
=======
This method is used to send a 200 ("Ok") response back down to the client indicating that everything worked out a-okay.  See the [`res.ok()` reference page](http://sailsjs.com/documentation/reference/response-res/res-ok) for usage info.

#### res.serverError()

This method is used to send a 500 ("Server Error") response back down to the client indicating that some kind of server error occurred.  See the [`res.serverError()` reference page](http://sailsjs.com/documentation/reference/response-res/res-server-error) for usage info.

#### res.badRequest()

This method is used to send a 400 ("Bad Request") response back down to the client indicating that the request is invalid.  See the [`res.badRequest()` reference page](http://sailsjs.com/documentation/reference/response-res/res-bad-request) for usage info.
>>>>>>> upstream/master

#### res.notFound()
这个方法用于发送一个404 ("Not Found")的响应返回给客户端，以告知请求的URL没有在服务器中找到任何匹配的路由。 💔 使用方法参考[`res.notFound()` reference page](http://sailsjs.org/documentation/reference/response-res/res-not-found)。

<<<<<<< HEAD
#### res.forbidden(message)
这个方法用于发送一个403 ("Forbidden")的响应返回给客户端，以告知请求不被允许。 🚫 使用方法参考[`res.forbidden()` reference page](http://sailsjs.org/documentation/reference/response-res/res-forbidden)。
=======
This method is used to send a 404 ("Not Found") response back down to the client indicating that the requested URL doesn&rsquo;t match any routes in the app.  See the [`res.notFound()` reference page](http://sailsjs.com/documentation/reference/response-res/res-not-found) for usage info.

#### res.forbidden()

This method is used to send a 403 ("Forbidden") response back down to the client indicating that the request is not allowed.  See the [`res.forbidden()` reference page](http://sailsjs.com/documentation/reference/response-res/res-forbidden) for usage info.

#### res.created()

This method is used to send a 201 ("Created") response back down to the client indicating that one or more new resources have been created. See the [`res.created()` reference page](http://sailsjs.com/documentation/reference/response-res/res-created) for usage info.
>>>>>>> upstream/master

<docmeta name="displayName" value="Default Responses">
