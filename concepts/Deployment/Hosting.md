# Hosting
下面是一份非全面的Sails.js主机提供商

##### 部署到Modulus?
+ http://blog.modulus.io/sails-js

##### 部署到OpenShift?
为了部署到Openshift，你需要在你的配置中做些主要的修改：打开你服务器的`config/local.js`文件，在这儿，你需要添加下面几行到文件中去：

```
	port: process.env.OPENSHIFT_NODEJS_PORT,
	host: process.env.OPENSHIFT_NODEJS_IP,
```

你还需要使用命令`npm i --save grunt-cli`安装`grunt-cli`。

做完这些之后，创建文件`.openshift/action_hooks/pre_start_nodejs`，并写入下面的内容：

```
#!/bin/bash
export NODE_ENV=production

if [ -f "${OPENSHIFT_REPO_DIR}"/Gruntfile.js ]; then
    (cd "${OPENSHIFT_REPO_DIR}"; node_modules/grunt-cli/bin/grunt prod)
fi
```
然后创建文件`/supervisor_opts`，并写入下面的内容。这个是为了告知Openshift的管理员忽略Sails的`.tmp`文件夹因为热加载功能。([source](https://gist.github.com/mdunisch/4a56bdf972c2f708ccc6#comment-1318102))

```
-i .tmp
```

你现在可以`git add . && git commit -a -m "your message" && git push`部署到openshift了。

##### 使用DigitalOcean?

+ https://www.digitalocean.com/community/articles/how-to-create-an-node-js-app-using-sails-js-on-an-ubuntu-vps
+ https://www.digitalocean.com/community/articles/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps
+ https://www.digitalocean.com/community/articles/how-to-host-multiple-node-js-applications-on-a-single-vps-with-nginx-forever-and-crontab

##### 部署到Heroku?

+ [Sails.js and Heroku](http://pburtchaell.com/2015/sails/)
+ [SailsCasts: Deploying a Sails App to Heroku](http://irlnathan.github.io/sailscasts/blog/2013/11/05/building-a-sails-application-ep26-deploying-a-sails-app-to-heroku/)
+ [Sails.js on Heroku](http://vort3x.me/sailsjs-heroku/)
+ https://groups.google.com/forum/#!topic/sailsjs/vgqJFr7maSY
+ https://github.com/chadn/heroku-sails
+ http://dennisrongo.com/deploying-sails-js-to-heroku
+ http://stackoverflow.com/a/20184907/486547

##### 部署到AWS?

+ http://blog.grio.com/2014/01/your-own-mini-heroku-on-aws.html
+ http://serverfault.com/questions/531560/creating-an-sails-js-application-on-aws-ami-instance
+ http://bussing-dharaharsh.blogspot.com/2013/08/creating-sailsjs-application-on-aws-ami.html
+ http://cloud.dzone.com/articles/how-deploy-nodejs-apps-aws-mac

##### 使用PM2?

+ http://devo.ps/blog/goodbye-node-forever-hello-pm2/

##### 部署到CloudControl?

+ https://www.cloudcontrol.com/dev-center/Guides/NodeJS/Sailsjs





<docmeta name="displayName" value="Hosting">

