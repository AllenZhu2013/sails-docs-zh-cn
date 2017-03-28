# 创建一个service
简单地在你的**api/services**文件夹下保存一个包含一个功能或者对象的JS文件。这个文件名将会被用于全局可访问的变量。比如，一个邮件服务也许是这样的：

```javascript
// EmailService.js - in api/services
module.exports = {

    sendInviteEmail: function(options) {
    
        var opts = {"type":"messages","call":"send","message":
            {
                "subject": "YourIn!",
                "from_email": "info@balderdash.co",
                "from_name": "AmazingStartupApp",
                "to":[
                    {"email": options.email, "name": options.name}
                ],
                "text": "Dear "+options.name+",\nYou're in the Beta! Click <insert link> to verify your account"
            }
        };
    
        myEmailSendingLibrary.send(opts);
        
    }
};
```

然后你可以在你的app中任何地方使用`EmailService`：

```javascript
// Somewhere in a controller
  EmailService.sendInviteEmail({email: 'test@test.com', name: 'test'});
```

<docmeta name="displayName" value="Creating a Service">
