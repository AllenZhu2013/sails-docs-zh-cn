# File Uploads
在Sails中上传文件类似于你在普通的Nodejs或者Express上传文件的做法。但是如果以前你使用的服务器平台是PHP、.NTE、Python、Ruby或Java的话，那可能有些不同于以前你使用的方法。但是不用担心：内核组已经尽最大的努力让上传动作保持可缩放和安全的情况变得容易实现。

Sails自带一个强大的“body分析器”叫做“Skipper”，该工具可以让实现流文件上传变得容易--不仅对于服务器的文件系统(也就是硬盘)，而且对于Amazon S3、MongoDb的gridfs或者其他任何支持的文件适配器。

### 上传一个文件
上传到HTTP web服务器的文件作为*file parameters*。同样地你可能发送一条表单POST到一个带有文本参数比如“name”，“email”和“password”的URL，你发送文件作为文件参数，就想“avatar”或“newSong”。

参考下面的例子：

```javascript
req.file('avatar').upload(function (err, uploadedFiles) {
  // ...
});
```

<<<<<<< HEAD
文件上传的动作应该在你的某一个控制器中的一个`action`中。下面的一个比较深入的例子来解释怎样允许用户上传一个avatar图片并将它关联到它的账户的。这假设你已经获取对服务器的访问控制权利，并且你存储已经登录的用户的ID在`req.session.me`。
=======
Files should be uploaded inside of an `action` in one of your controllers.  Here's a more in-depth example that demonstrates how you could allow users to upload an avatar image and associate it with their accounts.  It assumes you've already taken care of access control in a policy, that you're storing the id of the logged-in user in `req.session.me`, and that you've put the base URL in an [environment-dependent configuration value](http://sailsjs.com/documentation/concepts/configuration#?environmentspecific-files-config-env) called `sails.config.appUrl`.
>>>>>>> upstream/master

 ```javascript
// api/controllers/UserController.js
//
// ...


/**
 * Upload avatar for currently logged-in user
 *
 * (POST /user/avatar)
 */
uploadAvatar: function (req, res) {

  req.file('avatar').upload({
    // don't allow the total upload size to exceed ~10MB
    maxBytes: 10000000
  },function whenDone(err, uploadedFiles) {
    if (err) {
      return res.negotiate(err);
    }

    // If no files were uploaded, respond with an error.
    if (uploadedFiles.length === 0){
      return res.badRequest('No file was uploaded');
    }


    // Save the "fd" and the url where the avatar for a user can be accessed
    User.update(req.session.me, {

      // Generate a unique URL where the avatar can be downloaded.
      avatarUrl: require('util').format('%s/user/avatar/%s', sails.config.appUrl, req.session.me),

      // Grab the first file and use it's `fd` (file descriptor)
      avatarFd: uploadedFiles[0].fd
    })
    .exec(function (err){
      if (err) return res.negotiate(err);
      return res.ok();
    });
  });
},


/**
 * Download avatar of the user with the specified id
 *
 * (GET /user/avatar/:id)
 */
avatar: function (req, res){

  req.validate({
    id: 'string'
  });

  User.findOne(req.param('id')).exec(function (err, user){
    if (err) return res.negotiate(err);
    if (!user) return res.notFound();

    // User has no avatar image uploaded.
    // (should have never have hit this endpoint and used the default image)
    if (!user.avatarFd) {
      return res.notFound();
    }

    var SkipperDisk = require('skipper-disk');
    var fileAdapter = SkipperDisk(/* optional opts */);

    // set the filename to the same file as the user uploaded
    res.set("Content-disposition", "attachment; filename='" + file.name + "'");

    // Stream the file down
    fileAdapter.read(user.avatarFd)
    .on('error', function (err){
      return res.serverError(err);
    })
    .pipe(res);
  });
}

//
// ...
```


#### 上传的文件放在哪里？
当使用默认的`receiver`，文件上传到`myApp/.tmp/uploads/`目录下。你可以使用`dirname`选项重写这个目录。注意：当你调用`.upload()`函数和当你调用skipper-disk适配器的时候你需要提供dirname选项(所以你上传和下载的目录位置都是同一个)。

<<<<<<< HEAD
####上传到自定义的文件夹
在上面的例子中我们上传文件到 .tmp/uploads。所以如何配置才能上传文件到我们自定义的文件夹呢，比如”assets/images“。我们可以通过下面的例子添加选项到上传函数中来实现这个目的：
=======

#### Where do the files go?
When using the default `receiver`, file uploads go to the `.tmp/uploads/` directory.  You can override this using the `dirname` option.  Note that you'll need to provide this option both when you call the `.upload()` function AND when you invoke the skipper-disk adapter (so that you are uploading to and downloading from the same place.)


#### Uploading to a custom folder
In the above example we upload the file to .tmp/uploads. So how do we configure it with a custom folder, say ‘assets/images’. We can achieve this by adding options to upload function as shown below.
>>>>>>> upstream/master

```javascript
req.file('avatar').upload({
  dirname: require('path').resolve(sails.config.appPath, 'assets/images')
},function (err, uploadedFiles) {
  if (err) return res.negotiate(err);

  return res.json({
    message: uploadedFiles.length + ' file(s) uploaded successfully!'
  });
});
```

### 例子
#### 生成一个API
首先我们需要为serving/storing文件生成一个新的`API`。使用Sails的命令行工具生成：

```sh
$ sails generate api file

debug: Generated a new controller `file` at api/controllers/FileController.js!
debug: Generated a new model `File` at api/models/File.js!

info: REST API generated @ http://localhost:1337/file
info: and will be available the next time you run `sails lift`.
```

#### 实现控制器的Actions
让我们创建一个`index`动作来初始化文件上传和一个`upload`动作来接收文件。

 ```javascript

// api/controllers/FileController.js

module.exports = {

  index: function (req,res){

    res.writeHead(200, {'content-type': 'text/html'});
    res.end(
    '<form action="http://localhost:1337/file/upload" enctype="multipart/form-data" method="post">'+
    '<input type="text" name="title"><br>'+
    '<input type="file" name="avatar" multiple="multiple"><br>'+
    '<input type="submit" value="Upload">'+
    '</form>'
    )
  },
  upload: function  (req, res) {
    req.file('avatar').upload(function (err, files) {
      if (err)
        return res.serverError(err);

      return res.json({
        message: files.length + ' file(s) uploaded successfully!',
        files: files
      });
    });
  }

};
```

<<<<<<< HEAD
#### 它们往哪里去？
当使用默认的`receiver`，文件将上传到`myApp/.tmp/uploads/`目录。你可以在`upload`动作中做任何想做的事情。

#### 上传到一个自定义的文件夹
在上面的例子中我们可以上传文件到.tmp/uploads。所以我们该如何配置成一个自定义的目录呢，以`assets/images`为例。我们通过添加选项到上传函数中来实现这个目录，如下：

 ```javascript
req.file('avatar').upload({
  dirname: './assets/images'
},function (err, uploadedFiles) {
  if (err) return res.negotiate(err);

  return res.json({
    message: uploadedFiles.length + ' file(s) uploaded successfully!'
  });
});
```

## 更多阅读
=======
## Read more

>>>>>>> upstream/master
+ [Skipper docs](https://github.com/balderdashy/skipper)
+ [Uploading to Amazon S3](http://sailsjs.com/documentation/concepts/file-uploads/uploading-to-s-3)
+ [Uploading to Mongo GridFS](http://sailsjs.com/documentation/concepts/file-uploads/uploading-to-grid-fs)



<docmeta name="displayName" value="File Uploads">
