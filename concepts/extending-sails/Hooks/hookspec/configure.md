# `configure()`
`configure`特性提供了一种在[default对象](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/defaults.html)已经被应用到所有钩子之后还可以配置一个钩子的方法。等到一个自定的钩子的`configure()`函数运行的时候，所有用户级别的配置和内核钩子设置将会被合并到`sails.config`中。但是，这时候你*不应该*依赖于其他自定义的钩子的配置，因为自定义钩子的加载顺序是不能保证的。

`configure`应该被实现成一个没有参数的函数，并且不应该返回任何值。比如，下面的`configure`函数可以被一个钩子用来与一个远程API通信，并基于用户是否设置钩子的`ssl`属性为`true`来改变API的终结点。注意钩子的配置关键词`this.configKey`在`configure`中可用。

```
configure: function() {

   // If SSL is on, use the HTTPS endpoint
   if (sails.config[this.configKey].ssl == true) {
      sails.config[this.configKey].url = "https://" + sails.config[this.configKey].domain;
   }
   // Otherwise use HTTP
   else {
      sails.config[this.configKey].url = "http://" + sails.config[this.configKey].domain;
   }
}
```

`configure`的最大好处是所有钩子的`configure`函数保证在任何[initialize函数](http://sailsjs.org/documentation/concepts/extending-sails/Hooks/hookspec/initialize.html)之前运行；因为一个钩子的`initialize`函数可以在其他钩子的设置中检查。


<docmeta name="displayName" value=".configure()">
<docmeta name="stabilityIndex" value="3">
