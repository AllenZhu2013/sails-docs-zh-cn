# `routes`
`routes`特性允许在加载的时候一个自定义的钩子简单地绑定新的路由到一个Sails app中。如果实现了，`routes`应该是一个要么带有一个`before`关键词，要么带有一个`after`关键词或者两者皆有的对象。那些关键词的值反过来应该也是对象，其中关键词是[ route addresses](http://sailsjs.org/documentation/concepts/Routes/RouteTargetSyntax.html?q=route-address)，值是带有标准`(req, res, next)`参数的路由操作函数。任何指定在`before`对象的路由都会绑定在自定义的用户路由(定义在[sails.config.routes](http://sailsjs.org/documentation/reference/sails.config/sails.config.routes.html))和[blueprint routes](http://sailsjs.org/documentation/reference/blueprint-api?q=blueprint-routes)*之前*(定义在[ sails.config.routes]())。相反地，指定为`after`对象的路由将会绑定到自定义的用户路由和[blueprint routes]()*之后*。比如，考虑下面的`count-requests`钩子：

 ```javascript
module.exports = function (sails) {

   return {

      initialize: function(cb) {
         this.numRequestsSeen = 0;
         this.numUnhandledRequestsSeen = 0;
         return cb();
      },

      routes: {
         before: {
            'GET /*': function (req, res, next) {
               this.numRequestsSeen++;
               return next();
            }
        },
        after: {
            'GET /*': function (req, res, next) {
               this.numUnhandledRequestsSeen++;
               return next();
            }
        }
    };
};
```

这个钩子将会处理所有的请求通过在`before`对象中提供的函数，然后递增它的`numRequestsSeen`变量。它也会处理任何*未被处理的*请求通过在`after`对象中提供的函数--也就是任何没有通过一个自定义路由配置或者一个blueprint绑定到app的路由。

> 在钩子中这两个变量`sails.hooks["count-requests"].numRequestsSeen`和`sails.hooks["count-requests"].numUnhandledRequestsSeen`的设置对于在Sails app中的其他模块也是可用的


<docmeta name="displayName" value=".routes">
<docmeta name="stabilityIndex" value="3">
