# Services
<<<<<<< HEAD
Service可以认为是在你的应用中你想要使用在很多地方的功能库。比如，你也许有一个EmailService，它封装了一些默认的消息样板代码，你可以在你的应用中的众多地方使用它们。使用services的主要好处是它们可以全局化的--你不需要使用`require()`函数来访问它们。
=======

**Services** are stateless libraries of functions (called **helpers**) that you can use from anywhere in your Sails app.  For example, you might have an `EmailService` which tidily wraps up one or more helper functions so you can use them in more than one place within your application.  Services and their helpers are the best and simplest way to build reusable code in a Sails app.

Another benefit of using services in Sails is that they are *globalized*--you don't have to use `require()` to access them (although you can if you prefer.  And you can disable the automatic exposure of global variables in your app's configuration.)   By default, you can access a service and call its helpers (e.g. `EmailService.sendHtmlEmail()` or `EmailService.sendPasswordRecoveryEmail()`) from anywhere: within controller actions, from inside other services, in custom model methods, or even from command-line scripts.


### Building a service

Services are simple to work with, and you can do almost anything with them.  To help keep code maintainable, Sails apps use a set of strong conventions.  To learn more about service and helpers, **[start here](http://sailsjs.com/documentation/concepts/services/creating-a-service)**.

>>>>>>> upstream/master


### Notes

> For additional reference, also check out <a href="https://blog.sergiocruz.me/sailsjs-services-how-to-use-them-in-your-controllers/">this blog post</a>.



<docmeta name="displayName" value="Services">
