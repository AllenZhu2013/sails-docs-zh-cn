# `defaults`
`defaults`特性既可以在作为一个对象去实现也可以作为一个带有一个参数的函数并返回一个对象去实现(参考下面的"using `defaults` as a function")。这个你指定的对象将会被用来提供Sails默认的配置值。你应该使用这个特性去指定默认的钩子配置。比如，如果你正在创建一个与一个远程服务通信的钩子，你也许想要提供一个默认的域名和超时时间：

```
{
   myapihook: {
      timeout: 5000,
      domain: "www.myapi.com"
   }
}
```

如果一个`myapihook.timeou`值是通过一个Sails配置文件提供的，那么该值会被使用，否则它会默认为`5000`。

##### Namespacing your hook configuration
对于[工程钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks?q=types-of-hooks)，你应该在一个可以唯一识别你的钩子(比如上面的`myapihook`)的关键词下namespace你的钩子配置。对于[可安装的钩子](http://sailsjs.org/documentation/concepts/extending-sails/Hooks?q=types-of-hooks)，你应该使用特殊的`__configKey__`关键词去允许在你的钩子的终端用户去[去改变关键词配置](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/usinghooks.html?q=changing-the-way-sails-loads-an-installable-hook)如果需要的话。一个使用`__configKey__`的钩子的默认的关键词是该钩子的名字。比如，如果你创建了一个叫做`sails-hooks-myawesomehook`的钩子，并包含下面的`defaults`对象：

```
{
   __configKey__: {
      name: "Super Bob"
   }
}
```

那么默认它会为`sails.config.myawesomehook.name`值提供默认的设置。如果钩子的终端用户重写钩子的名字为`foo`，那么`defaults`对象将会为`sails.config.foo.name`提供一个默认值。

##### 将defults作为函数使用
如果你为`defaults`特性指定一个函数而不是一个对象，那么它会带有一个参数(`config`)，该参数会接收任何Sails配置的重写。配置重写会在Sails启动的时候通过传递设置到命令行中生效。(比如`sails lift --prod`)，也可以当可编程地启动或者加载Sails的时候通过传递一个对象作为第一个参数(比如`Sails.lift({port: 1338}, ...)`)或者通过使用一个[.sailsrc](http://sailsjs.org/documentation/anatomy/myApp/sailsrc.html)文件。`defaults`函数应该返回一个代表你的钩子的默认配置的对象。


<docmeta name="displayName" value=".defaults">
<docmeta name="stabilityIndex" value="3">
