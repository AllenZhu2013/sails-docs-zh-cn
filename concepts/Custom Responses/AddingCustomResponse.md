# 添加自定义的响应

为了添加你自定义的响应方法，可以简单地在`/api/responses`中添加一个文件，该文件的名称需要和你想要创建的方法名称一样。这个文件应该导出一个函数，该函数可以带任何你想要的参数。


```
/** 
 * api/responses/myResponse.js
 *
 * This will be available in controllers as res.myResponse('foo');
 */

module.exports = function(message) {
   
  var req = this.req;
  var res = this.res;
   
  var viewFilePath = 'mySpecialView';
  var statusCode = 200;

  var result = {
    status: statusCode
  };

  // Optional message
  if (message) {
    result.message = message;
  }

  // If the user-agent wants a JSON response, send json
  if (req.wantsJSON) {
    return res.json(result, result.status);
  }

  // Set status code and view locals
  res.status(result.status);
  for (var key in result) {
    res.locals[key] = result[key];
  }
  // And render view
  res.render(viewFilePath, result, function(err) {
    // If the view doesn't exist, or an error occured, send json
    if (err) {
      return res.json(result, result.status);
    }

    // Otherwise, serve the `views/mySpecialView.*` page
    res.render(viewFilePath);
  });   
```
<docmeta name="displayName" value="Adding a Custom Response">
