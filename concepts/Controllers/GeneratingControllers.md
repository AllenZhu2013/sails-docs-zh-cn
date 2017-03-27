# 生成控制器
你可以使用[Sails命令行工具](http://sailsjs.org/documentation/reference/cli)快速生成一个控制器，只需要输入：

<<<<<<< HEAD
 ```sh
=======
You can use the [Sails command line tool](http://sailsjs.com/documentation/reference/command-line-interface) to quickly generate a controller, by typing:

```sh
>>>>>>> upstream/master
$ sails generate controller <controller name> [action names separated by spaces...]
```

比如，如果你输入如下命令：

```sh
$ sails generate controller comment create destroy tag like
info: Generated a new controller `comment` at api/controllers/CommentController.js!
```

Sails将会生成`api/controllers/CommentController.js`:

```javascript
/**
 * CommentController.js
 *
 * @description :: Server-side logic for managing comments.
 */

module.exports = {

  /**
   * CommentController.create()
   */
  create: function (req, res) {
    return res.json({
      todo: 'Not implemented yet!'
    });
  },

  /**
   * CommentController.destroy()
   */
  destroy: function (req, res) {
    return res.json({
      todo: 'Not implemented yet!'
    });
  },

  /**
   * CommentController.tag()
   */
  tag: function (req, res) {
    return res.json({
      todo: 'Not implemented yet!'
    });
  },

  /**
   * CommentController.like()
   */
  like: function (req, res) {
    return res.json({
      todo: 'Not implemented yet!'
    });
  }
};
```


<docmeta name="displayName" value="Generating Controllers">
