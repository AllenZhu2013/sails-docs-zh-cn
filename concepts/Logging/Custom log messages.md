# 自定义日志消息
从你的应用代码中发射自定义的消息或事件是很有帮助的。无论你是在追踪在后台程序发送的邮件状态，还是仅仅查找一种在你的应用代码中调用[`console.log()`](https://nodejs.org/api/console.html#console_console_log_data)的另外方法。

为了便利，Sails暴露了它的内部日志接口为`sails.log`。它的使用方法非常类似于Node的`console.log()`，但是还有一些额外的有用特性；换句话说就是支持多种带着彩色和前缀的终端输出的日志等级记录。

参考信息和例子请参考[sails.log()](http://sailsjs.org/documentation/reference/application/sails-log),或配置选项参考[sails.config.log](http://sailsjs.org/documentation/reference/configuration/sails-config-log)。

<<<<<<< HEAD
=======
See [sails.log()](http://sailsjs.com/documentation/reference/application/sails-log) for more information and examples, or  [sails.config.log](http://sailsjs.com/documentation/reference/configuration/sails-config-log) for configuration options.
>>>>>>> upstream/master

## 可用的方法
下面的每一种方法都接受一组不限数目的不限类型的参数，通过逗号分隔区分。就像`console.log`一样，传递给Sails日志器的数据作为参数被自动地修饰，以便可以使用Node的[`util.inspect()`](http://nodejs.org/api/util.html#util_util_inspect_object_options)读取。因此这是一个标准的Nodejs会话应用;*任何*词语、错误、日期、数组或绝大部分的数据类型都可以使用内建的[`util.inspect()`](https://nodejs.org/api/util.html#util_util_inspect_object_options)逻辑很好地打印(比如，你可以看到{ pet: { name: 'Hamlet' } }而不是简单的打印[`object Object`])。同时如果你使用`inspect()`方法记录一个对象，它将会自动运行并且它返回的字符串将会被写到控制台。


### sails.log.error()
将会使用“error”的打印级别写控制台输出到`stderr`。用于追踪主要错误。

```js
sails.log.error('Sending 500 ("Server Error") response.');
// -> error: Sending 500 ("Server Error") response.
```

### sails.log.warn()
将会使用“warn”的打印级别写控制台输出到`stderr`。用于追踪失败的操作信息。

```js
sails.log.warn('File upload quota exceeded for user #%d.  Request aborted.', user.id);
// -> warn: File upload quota exceeded for user #94271.  Request aborted.
```

### sails.log()

_aka sails.log.debug()_

默认的打印函数，将会使用“debug”的打印级别写控制台输出到`stderr`。用于在你团队中传递重要的技术信息或者作为`console.log()`的第二方案。

```js
sails.log('This endpoint (`POST /accounts`) will be deprecated in the next few days.  Please use `POST /signup` instead. ');
// -> debug: This endpoint (`POST /accounts`) will be deprecated in the next few days.  Please use `POST /signup` instead.
```

### sails.log.info()
将会使用“info”的打印级别写控制台输出到`stdout`。用于捕捉关于你的app的业务逻辑的信息。

```js
sails.log.info('A new user (', newUser.emailAddress, ') just signed up!');
// -> info: A new user ( irl@foobar.com ) just signed up!
```

### sails.log().verbose()
将会使用“verbose”的打印级别写控制台输出到`stdout`。对于捕捉你的app中详细的信息比较有用，你可能在少数的一些场合下会使用这种打印。

```js
sails.log.verbose('A user (IP adddress: `%s`) initiated an account transfer...', req.ip);
// -> verbose: A user (IP adddress: `10.48.1.191`) initiated an account transfer...
```


### sails.log.silly()
将会使用“silly”的打印级别写控制台输出到`stdout`。对于捕捉那些绝顶荒谬的信息比较有用，使用的场合也比较少。

```js
sails.log.silly(
'Successfully fetched Account record for requesting authenticated user (`%d`).',
'Took %dms.', req.param('id'), msElapsed);
// -> silly: Successfully fetched Account record for authenticated user (`49722`). Took 41ms.
```




<docmeta name="displayName" value="Custom log messages">

