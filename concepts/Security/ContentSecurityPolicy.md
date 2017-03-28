# 内容安全策略(CSP)
Content Security Policy(CSP)是一哥W3C规范，用来指示客户端浏览器允许加载哪些类型和哪个位置的资源。该文档使用“指令助记符”开定义一个为目标资源类型加载行为。指令助记符能够使用HTTP响应头部或者HTML Meta 标签来指定。

#### HTTP头部
| Header                    | Browsers                                                                               |
| ------------------------- | -------------------------------------------------------------------------------------- |
| Content-Security-Policy   | (W3C Standard header) Chrome version >= 25, Firefox version >= 23, Opera version >= 19 |
| X-Content-Security-Policy | Firefox version < 23, IE version 10                                                    |
| X-WebKit-CSP              | Chrome version < 25                                                                    |

#### Supported Directives
| Directive     | |
|---------------|--------------------------|
| default-src   | Loading policy for all resources type in case a resource type dedicated directive is not defined (fallback) |
| script-src    | Defines which scripts the protected resource can execute |
| object-src    | Defines from where the protected resource can load plugins |
| style-src     | Defines which styles (CSS) the user applies to the protected resource |
| img-src       | Defines from where the protected resource can load images |
| media-src     | Defines from where the protected resource can load video and audio |
| frame-src     | Defines from where the protected resource can embed frames |
| font-src      | Defines from where the protected resource can load fonts |
| connect-src   | Defines which URIs the protected resource can load using script interfaces |
| form-action   | Defines which URIs can be used as the action of HTML form elements |
| sandbox       | Specifies an HTML sandbox policy that the user agent applies to the protected resource |
| script-nonce  | Defines script execution by requiring the presence of the specified nonce on script elements |
| plugin-types  | Defines the set of plugins that can be invoked by the protected resource by limiting the types of resources that can be embedded |
| reflected-xss | Instructs a user agent to activate or deactivate any heuristics used to filter or block reflected cross-site scripting attacks, equivalent to the effects of the non-standard X-XSS-Protection header |
| report-uri    | Specifies a URI to which the user agent sends reports about policy violation |

> 更多资料请参考[W3C CSP Spec](https://w3c.github.io/webappsec/specs/content-security-policy/)。

> For more information, see the [W3C CSP Spec](https://w3c.github.io/webappsec/specs/content-security-policy/)





<docmeta name="displayName" value="Content Security Policy">

