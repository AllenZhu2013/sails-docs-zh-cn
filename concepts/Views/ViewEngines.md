# 模板引擎
默认的模板引擎是[EJS](https://github.com/visionmedia/ejs)。

##### swapping out the view engine
为了使用不同的模板引擎，你应该在你的工程中使用npm安装，然后在[`config/views.js`](http://sailsjs.org/documentation/anatomy/myApp/config/views.js.html)中设置`sails.config.views.engine`。

比如，为了切换到Jade，运行`run npm install jade --save-dev`，然后在[`config/views.js`](http://sailsjs.org/documentation/anatomy/myApp/config/views.js.html)中设置`engine: 'jade'`。

##### 模板引擎
- [atpl](https://github.com/soywiz/atpl.js)
  - [dust](https://github.com/akdubya/dustjs) [(website)](http://akdubya.github.com/dustjs/) (.dust)
  - [eco](https://github.com/sstephenson/eco)
  - [ect](https://github.com/baryshev/ect) [(website)](http://ectjs.com/)
  - [ejs](https://github.com/visionmedia/ejs) (.ejs)
  - [haml](https://github.com/visionmedia/haml.js) [(website)](http://haml.info/)
  - [haml-coffee](https://github.com/9elements/haml-coffee) [(website)](http://haml.info/)
  - [handlebars](https://github.com/wycats/handlebars.js/) [(website)](http://handlebarsjs.com/) (.hbs)
  - [hogan](https://github.com/twitter/hogan.js) [(website)](http://twitter.github.com/hogan.js/)
  - [jade](https://github.com/visionmedia/jade) [(website)](http://jade-lang.com/) (.jade)
  - [jazz](https://github.com/shinetech/jazz)
  - [jqtpl](https://github.com/kof/node-jqtpl) [(website)](https://github.com/kof/jqtpl)
  - [JUST](https://github.com/baryshev/just)
  - [liquor](https://github.com/chjj/liquor)
  - [lodash](https://github.com/bestiejs/lodash) [(website)](http://lodash.com/)
  - [mustache](https://github.com/janl/mustache.js)
  - [QEJS](https://github.com/jepso/QEJS)
  - [ractive](https://github.com/Rich-Harris/Ractive)
  - [swig](https://github.com/paularmstrong/swig) [(website)](http://paularmstrong.github.com/swig/)
  - [templayed](http://archan937.github.com/templayed.js/)
  - [toffee](https://github.com/malgorithms/toffee)
  - [underscore](https://github.com/documentcloud/underscore) [(website)](http://documentcloud.github.com/underscore/)
  - [walrus](https://github.com/jeremyruppel/walrus) [(website)](http://documentup.com/jeremyruppel/walrus/)
  - [whiskers](https://github.com/gsf/whiskers.js)

##### 添加新的自定义的模板引擎
如何添加一个模板引擎的支持没有罗列在上面的列表中，不过你可以查看这个链接地址：[consolidate project](https://github.com/visionmedia/consolidate.js/blob/master/Readme.md#api)



<docmeta name="displayName" value="View Engines">
