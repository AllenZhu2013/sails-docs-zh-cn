# CSRF
跨站请求伪造(Cross-site request forgery[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)))是一种类型的攻击，该攻击会强制一个终端用户去执行一个它当前认证的web应用后端的不想要的动作。换句话说，如果没有保护，存储在诸如Chrome浏览器中的cookie将会被用于发送请求给Chase.com从一个用户的电脑，不论哪个用户正在访问Chase.com还是Horrible-Hacker-Site.com。

### 使能CSRF保护
Sails绑定了一些立即可用的可选CSRF保护。为了使能这些内建的措施，只需要调整[sails.config.csrf](http://sailsjs.org/documentation/reference/Configuration/CSRF.html)中下面的配置(也可以修改文件[`config/csrf.js`](http://sailsjs.org/documentation/anatomy/myApp/config/csrf.js.html))：

```js
csrf: true
```

注意如果你已经存在通过POST、PUT、DELETE请求来与你的Sails后端通信的代码时，你需要获取一个CSRF令牌并将它作为一个参数或者头部包含到请求中去。更多精彩,尽在一秒。

### CSRF令牌
就像大部分的NODE应用，Sails和Express兼容Connect的[CSRF protection middleware](http://www.senchalabs.org/connect/csrf.html)以保证对抗这样的攻击。这个中间件实现了[Synchronizer Token Pattern](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)。当CSRF保护使能的时候，所有到Sails的非GET的请求都必须携带有CSRF令牌，可以通过在一个请求字符串中或HTTP body中的一个参数或者是头部来认证。、

CSRF令牌是临时性的且是会话专属的；比如想象Mary和Muhammad都是访问运行Sails的电商网站的购物者，并且CSRF开启。那我们假设说在周一，Mary和Muhammad都同时购物。为了做到这个，我们的网站需要分发至少两个不同的CSRF令牌-一个给Mary另外一个给Muhammad。之后如果我们的网站后台接收到没有或者不正确的CSRF令牌的请求，那么将会拒绝这个请求。现在我们可以确信当Mary导航去玩在线扑克，那么第三方网站无法欺骗浏览器使用她的cookie去发送恶意的请求给我们的网站。

### 分配CSRF令牌
为了获得CSRF令牌，你既可以在你的视图中使用[locals](http://sailsjs.org/documentation/concepts/Views/Locals.html)来自举(这个对于传统的web应用程序比较好)也可以使用sockets或者从一个特殊被保护的JSON终端的AJAX来获取(这个对于单页面的web应用比较便利)。

##### 使用本地视图
对于老式的表单提交，从一个视图中传递数据给一个表单动作是很容易的。你可以抓取你的视图中令牌，它一般是在本地视图标签`<%= _csrf %>`中获取。

比如：

```html
<form action="/signup" method="POST">
 <input type="text" name="emailaddress"/>
 <input type='hidden' name='_csrf' value='<%= _csrf %>'>
 <input type='submit'>
</form>
```

如果你在做一个带有表单的`multipart/form-data`上传，确保在`file`输入之前放置`_csrf`字段，否则你有可能有超时的风险并且在文件结束上传完成之前得到403的错误代码。

##### 使用AJAX/Websockets
在AJAX/Socket-heavy的app中，你也许倾向于发送GET请求到内建的`/csrfToken`路由，该路由会返回JSON数据，比如：

```json
{
  "_csrf": "ajg4JD(JGdajhLJALHDa"
}
```

### Spending CSRF Tokens
一旦你使能CSRF保护，任何POST、PUT、DELETE请求(**包含**虚拟请求，比如来自socket.io)到你的Sails app的都需要发送一个随同的CSRF令牌作为头部或者参数。否则，它们都会被拒绝并放回403错误。

比如，如果你使用jQuery从网页中发送一条AJAX请求：

```js
$.post('/checkout', {
  order: '8abfe13491afe',
  electronicReceiptOK: true,
  _csrf: 'USER_CSRF_TOKEN'
}, function andThen(){ ... });
```

对于大部分的客户端模块，你也许不会访问到自己的AJAX请求。在这个例子中，你可以考虑在你的请求中在URL中直接发送CSRF令牌。然而，如果你这样做了，记得在实现之前对令牌做URL编码操作：

```js
..., {
  checkoutAction: '/checkout?_csrf='+encodeURIComponent('USER_CSRF_TOKEN')
}
```

### 注意
> + 你可以选择发送CSRF令牌为`X-CSRF-Token`头部而不是`_csrf`的参数。
> + 对于大部分开发者和组织，如果你允许用户登录或者从浏览器可以安全地访问你的Sails的话CSRF的攻击没必要那么关注。如果做不到的话(比如用户从你的native iOS或安卓 app只能访问那安全的部分)，也有可能你不需要使能你的CSRF保护。为什么呢？因为从技术上来说，本页面讨论的CSRF攻击只是用户使用相同的客户端(比如Chrome)应用访问不同的web服务(比如 Chase.com, Horrible-Hacker-Site.com)的场景下才可能发生。
> + 关于CSRF更多的信息，参考[Wikipedia](http://en.wikipedia.org/wiki/Cross-site_request_forgery)；
> + 在传统的表单提交中对于“spending” CSRF 令牌，参考上面的例子(在"Using View Locals"中)；


<docmeta name="displayName" value="CSRF">
