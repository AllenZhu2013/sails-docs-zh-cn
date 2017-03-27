# Sails + Passport的认证与权限
通行证的方式在Sails中工作得很好！总之，因为Sails使用Connect/Express作为它的内核，所以所有面向Connect/Express的事情都可以工作得很好。实际上，Sails对于拦截大部分的Express中间件来配合socket.io工作没有任何问题。

<<<<<<< HEAD

#### 使用passport.js的社区支持的Sails扩展

+ [sails-auth](https://www.npmjs.com/package/sails-auth): Passport-based认证扩展，包含Basic Auth。
+ [sails-permissions](https://www.npmjs.com/package/sails-permissions): 给sails.js使用的权限和权利：支持用户使用passport.js进行用户认证，role-based权限，以及对象所有权、列层级安全性。
+ [Tutorial on how to implement passport.js with sails.js](http://www.geektantra.com/2013/08/implement-passport-js-authentication-with-sails-js/).
+ [Waterlock](http://waterlock.ninja/): 一个全包含用户认证/json web令牌管理工具，为Sails构建的。
=======
Passport works with Sails just like it does with Express.  Since Sails uses Connect/Express at its core, all of the Connect/Express-oriented things work pretty well.  In addition, Sails has no problem interpreting most Express middleware to work with socket.io.

In most cases, Passport is overkill for Sails apps, since it is straightforward to implement a simple authentication system.  For example:
>>>>>>> upstream/master

+ [Sails.js in Action](http://sailsjs.com/book)
+ [Chp 15 Repo](https://github.com/sailsinaction/brushfire-ch15-end)
+ [Sails 101: Basic Login](https://github.com/sails101/basic-login)

Passport is, however, quite useful if you plan on integrating with many different social login providers (~4 or more).  For example:

+ [Sails 101: Sails with Passport.js](https://github.com/sails101/using-passport)
+ [Another tutorial on how to implement Passport.js with Sails](http://iliketomatoes.com/implement-passport-js-authentication-with-sails-js-0-10-2/).
+ [Waterlock](http://waterlock.ninja/): A user authentication/json web token (JWT) management tool, built for Sails.  Supports Twitter, Facebook, and Google Auth.


<docmeta name="displayName" value="Sails + Passport">
