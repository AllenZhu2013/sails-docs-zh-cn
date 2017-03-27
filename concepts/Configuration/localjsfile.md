# `config/local.js`文件
你可以使用`config/local.js`文件来为你的本地环境(比如你的电脑)做配置。在这个文件中的设置的优先级将会高于其他配置文件除了[.sailsrc](http://sailsjs.org/documentation/concepts/Configuration/usingsailsrcfiles.html)。因为它的本身是为了本地使用，所以这个文件不应该归入版本管理库中(这也是将此文件加入到`.gitignore`的原因)。你可以使用`local.js`来存储本地数据库的设置，改变你的电脑中服务器运行的端口号等等。

<<<<<<< HEAD
当你在开发你的app的时候，这个配置文件应该包含所有专用于你的开发电脑或者服务器的任何配置。如果你使用git，默认这个文件是包含在`.gitignore`文件中，所以当你提交代码的时候这个文件不会上传到远程代码库中。
=======
The `config/local.js` file is useful for configuring a Sails app for your local environment (your laptop, for example).  The settings in this file take precedence over all other config files except [.sailsrc](http://sailsjs.com/documentation/concepts/Configuration/usingsailsrcfiles.html).  Since they're intended only for local use, they should not be put under version control (and are included in the default `.gitignore` file for that reason).  Use `local.js` to store local database settings, change the port used when lifting an app on your computer, etc.
>>>>>>> upstream/master

当你准备部署你的app在产品环境下的时候，你也可以使用这个文件在你部署的服务器中配置一些选项。然而，对于服务器部署，更应该考虑使用环境变量。你也可以为你的本地环境使用命令行参数和`.sailsrc`文件作为替代办法来设置你的本地开发配置。关于Sails配置的详细信息参考刚才的概述部分。

<<<<<<< HEAD
> **注意：** 这个文件包含在你的.gitignore中，所以当你使用git作为版本控制工具的时候，牢记该文件将不会被提交到你的代码库中。好消息是这意味着你可以为你的本地环境在这个文件中指定你自己的配置而不会疏忽地将个人信息提交到代码库中去。另外，也阻止了你团队中的其他成员提交他们自己改动的本地配置。
=======
When you&rsquo;re ready to deploy your app in production, you can also use this file for configuration options on the server where it will be deployed.  However, for server deployments, environment variables are usually preferable.  You can also use command-line arguments and the `.sailsrc` file as alternatives to `config/local.js` for your local development configuration. [See the overview](http://sailsjs.com/documentation/concepts/Configuration) for general information about Sails configuration.

> **Note:** This file is included in your .gitignore, so if you&rsquo;re using git as a version control solution for your Sails app, keep in mind that this file won&rsquo;t be committed to your repository!
> Good news is, that means you can specify configuration for your local machine in this file without inadvertently committing personal information (like database passwords) to the repo.  Plus, this prevents other members of your team from commiting their local configuration changes on top of yours.
>>>>>>> upstream/master

<docmeta name="displayName" value="The local.js file">
