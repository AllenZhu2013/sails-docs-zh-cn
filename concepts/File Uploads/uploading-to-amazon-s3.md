# 上传到Amazon S3
> 请注意你的Amazon S3的块必须创建在'US Standard'域中。如果你没有这么做的话，将会得到错误”TypeError('Uncaught, unspecified "error" event.')“。

在Sails中。只需要做些额外的改动便可以支持你上传文件到Amazon S3。

首先安装[S3文件适配器](https://github.com/balderdashy/skipper-s3)：

```sh
$ npm install skipper-s3 --save
```

然后在你其中的一个控制器中使用：

```javascript
  uploadFile: function (req, res) {
    req.file('avatar').upload({
      adapter: require('skipper-s3'),
      key: 'S3 Key'
      secret: 'S3 Secret'
      bucket: 'Bucket Name'
    }, function (err, filesUploaded) {
      if (err) return res.negotiate(err);
      return res.ok({
        files: filesUploaded,
        textParams: req.params.all()
      });
    });
  }
```

<docmeta name="displayName" value="Uploading to S3">
