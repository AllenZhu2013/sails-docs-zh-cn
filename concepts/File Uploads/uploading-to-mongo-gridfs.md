# 上传到Mongo GridFS
因为Mongo的GridFS文件系统的支持才让上传文件到MongoDb变成可能。在Sails，只需要使用为[MongoDb的GridFs](https://github.com/willhuang85/skipper-gridfs)的Skipper适配器做些额外的改动便可以实现这个：

首先安装：

```sh
$ npm install skipper-gridfs --save
```

然后在你的控制器中使用：

```javascript
  uploadFile: function (req, res) {
    req.file('avatar').upload({
      adapter: require('skipper-gridfs'),
      uri: 'mongodb://[username:password@]host1[:port1][/[database[.bucket]]'
    }, function (err, filesUploaded) {
      if (err) return res.negotiate(err);
      return res.ok();
    });
  }
```

<docmeta name="displayName" value="Uploading to GridFS">
