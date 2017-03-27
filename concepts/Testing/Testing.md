# Testing
## 准备工作
对于我们的测试组件，我们使用[mocha](http://mochajs.org/)。在你开始构建你的测试用例之前，你应该首先组织你的`./test`目录结构，比如下面的例子：

```batch
./myApp
├── api/
├── assets/
├── ...
├── test/
│  ├── integration/
│  │  ├── controllers/
│  │  │  └── UserController.test.js
│  │  ├── models/
│  │  │  └── User.test.js
│  │  └── ...
│  ├── fixtures/
|  ├── ...
│  ├── bootstrap.test.js
│  └── mocha.opts
└── views/

```
    
### bootstrap.test.js
在你运行你的测试之前和之后想要执行一些代码的话这个文件很有用。因为在app启动后你的模型被转换为Waterline集合，所以在尝试测试它们之前需要启动你的SailsApp(这个同样地使用于控制器和你的app中的其他部分，所以确保首先调用这个文件)：

```javascript
var sails = require('sails');

before(function(done) {

  // Increase the Mocha timeout so that Sails has enough time to lift.
  this.timeout(5000);

  sails.lift({
    // configuration for testing purposes
  }, function(err) {
    if (err) return done(err);
    // here you can load fixtures, etc.
    done(err, sails);
  });
});

after(function(done) {
  // here you can clear fixtures, etc.
  sails.lower(done);
});
```

### mocha.opts
这个文件应该包含[mocha.opts](http://mochajs.org/#mocha-opts)描述的mocha配置选项。

<<<<<<< HEAD
注意:如果你的测试代码是CoffeeScript编写的，请确保添加以下代码到你的`mocha.opts`中：
=======
This file should contain mocha configuration as described here: [mocha.opts](https://mochajs.org/#mochaopts)
>>>>>>> upstream/master

```
--require coffee-script/register
--compilers coffee:coffee-script/register
```

**注意**：在Mocha中默认的测试超时时间是2秒。在mocha.opt中提高超时时间确保Sails完全启动在任何测试case开始之前，比如：

```
--timeout 5s
```

## 写Cases
一旦你准备好你的目录之后你可以开始写你的单元测试。

比如：./test/unit/models/Users.test.js
```js
describe('UserModel', function() {

  describe('#find()', function() {
    it('should check find function', function (done) {
      User.find()
      .then(function(results) {
        // some tests
        done();
      })
      .catch(done);
    });
  });

});
```

#### 测试控制器
为了测试控制器响应你可以使用[Supertest](https://github.com/visionmedia/supertest)库，该库提供一些有用的方法来测试HTTP请求。

./test/unit/controllers/UsersController.test.js

```js
var request = require('supertest');

describe('UserController', function() {

  describe('#login()', function() {
    it('should redirect to /mypage', function (done) {
      request(sails.hooks.http.app)
        .post('/users/login')
        .send({ name: 'test', password: 'test' })
        .expect(302)
        .expect('location','/mypage', done);
    });
  });

});
```

## 运行测试
为了使用`mocha`运行你的测试，你可以在命令行使用mocha命令然后传递任何你想跑的测试参数，确保在复位你的测试之前先调用bootstrap.test.js，如下：

`mocha test/bootstrap.test.js test/unit/**/*.test.js`

#### 使用npm测试来运行你的测试
为了避免输入mocha命令，就像之前声明过的一样(特别地当调用bootstrap.test.js的时候)并使用`npm test`命令，你需要修改你的package.json。在Script obj中，添加一个`test`关键词然后将这个字符串`mocha test/bootstrap.test.js test/unit/**/*.test.js`作为它的值：

```js
  // package.json
  "scripts": {
    "start": "node app.js",
    "debug": "node debug app.js",
    "test": "node ./node_modules/mocha/bin/mocha test/bootstrap.test.js test/integration/**/*.test.js"
  }
```
 
`*`是一个通配符去匹配所有包含在`integration/`文件夹下的所有以`test.js`结束的文件，所以如果它特别适合你，你可以优先修改它去搜索`*.spec.js`的文件。同理你可以使用两个通配符`*`而不是一个去匹配你的文件夹。

<docmeta name="displayName" value="Testing">
