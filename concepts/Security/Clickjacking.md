# 点击劫持
[点击劫持](https://www.owasp.org/index.php/Clickjacking)(也称UI-覆盖攻击)，攻击者诱导用户去点击触发那些“无意识的”UI事件(比如DOM事件)。

### X-FRAME-OPTIONS
阻止点击劫持攻击的一个最简单的办法是去使能X-FRAME-OPTIONS头部。

##### 使用[lusca](https://github.com/krakenjs/lusca#luscaxframevalue)
`lusca`是在[Apache证书](https://github.com/krakenjs/lusca/blob/master/LICENSE.txt)下的开源软件。

```sh
# In your sails app
npm install lusca --save
```

然后在`config/http.js`中的`中间件`配置对象中配置：

```js
  // ...
  // maxAge ==> Number of seconds strict transport security will stay in effect.
  xframe: require('lusca').xframe('SAMEORIGIN'),
  // ...
  order: [
    // ...
    'xframe'
    // ...
  ]
```

### 额外的资源

+ [Clickjacking (OWasp)](https://www.owasp.org/index.php/Clickjacking)




<docmeta name="displayName" value="Clickjacking">
<docmeta name="tags" value="clickjacking,ui redress attack">
