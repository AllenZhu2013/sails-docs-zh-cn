![Squiddy reads the docs](http://sailsjs.org/images/squidford_swimming.png)

# Sails.js文档

当前Sails的稳定版本的官方文档是在[master branch](github.com/balderdashy/sails-docs)这个代码库上。[Sails官方网站](http://sailsjs.org)上的大部分章节内容都可以在这里找得到。


## 其他语言版本

Sails文档已经被翻译成多种其他语言版本。下面的列表是对应的语言翻译工程的链接。

| 语言                     | [IETF语言标签](https://en.wikipedia.org/wiki/IETF_language_tag)  | 维护者        | 代码库                               |
| ---------------------------- | ------- | ------------------ | ---------------------------------- |
| Japanese                     | `ja`    | [@kory-yhg](https://github.com/kory-yhg)      | [sails-docs-ja](https://github.com/balderdashy/sails-docs/tree/ja) <br/>(_live on [sailsjs.jp](http://sailsjs.jp)_)
| Spanish                      | `es`    | [@eduartua](https://github.com/eduartua/) & [@alejandronanez](https://github.com/alejandronanez)   | [sails-docs-es](https://github.com/eduartua/sails-docs-es)
| Brazilian Portuguese         | `pt-BR` | [@marceloboeira](https://github.com/marceloboeira) & [@gabrielalmir10](https://github.com/gabrielalmir10)   | [sails-docs-pt-BR](https://github.com/balderdashy/sails-docs/tree/pt-BR)
| Taiwanese Mandarin           | `zh-TW` | [@CalvertYang](https://github.com/CalvertYang)   | [sails-docs-zh-TW](https://github.com/balderdashy/sails-docs/tree/zh-TW)
| Korean                       | `ko`    | [@sapsaldog](https://github.com/sapsaldog)   | [sails-docs-ko](https://github.com/balderdashy/sails-docs/tree/ko)
| Chinese                      | `zh-cn`    | [@linxiaowu66](https://github.com/linxiaowu66)   | [sails-docs-zh-cn](https://github.com/linxiaowu66/sails-docs-zh-cn)

> 因为我们正在使用分支来追朔Sails文档的不同版本，所以我们正在逐渐地放弃掉将不同语言版本作为分支的原始做法。在你着手开始一项新的翻译工程之前，我们希望你细看一下[下面更新的内容](#how-can-i-help-translate-the-documentation)-- 流程上稍微有一点点变化。



## Sails文档的贡献工作

我们非常欢迎你的帮助！  请提出一个带有更正错误或者添加新的东西的pull请求到**master**然后我们会再三检查之后尽快合并到主干分支上去。

其次，我们欢迎你提出关于我们正在使用的管理文档的流程和社区工作的细则的建议。请将你的想法发送到Google Group-或者如果你想要获得直接的帮助，请在Twitter上联系@fancydoilies, @rudeboot，或@mikermcneil。

#### 我应该编辑哪个分支？

这取决于你想要编辑的类型。大部分情况下，你都会编辑Sails最新稳定版本的相关文档(也就是在[NPM](npmjs.org/package/sails)上的版本)同时你也会想要编辑这个代码库的`master`(也就是你在sails-docs代码库上默认看到的分支)。

另一方面，如果你想要在一个未来版本中编辑一个没有加入版本中的特性；或者更常见的，作为一个提议的特性或者到Sails或一个相关工程的开放的pull请求的伴随物，那么你接下来将会编辑那些Sails没有发布的版本(有时候我们称之为“边缘版本”)。


| Branch (in `sails-docs`)                    | Documentation for Sails Version...                                   | Preview At...      |
|-------------------------------------------------------------------------------------|------------------------|:-------------------|
| [`master`](https://github.com/balderdashy/sails-docs/tree/master) | [![NPM version](https://badge.fury.io/js/sails.png)](http://badge.fury.io/js/sails) | [preview.sailsjs.org](http://preview.sailsjs.org)
| [`1.0`](https://github.com/balderdashy/sails-docs/tree/1.0) | Upcoming v1.0 release _(branch not available yet)_           | [next.sailsjs.org](http://next.sailsjs.org)
| [`0.11`](https://github.com/balderdashy/sails-docs/tree/0.11) | Sails v0.11.x           | [0.11.sailsjs.org](http://0.11.sailsjs.org)


#### 这些文档是怎样被编译并推送到网站的？

我们使用一种叫做`doc-templater`的模块来转换.md文件为html文件。你可以参考该链接来学习该模块是如何工作的-[doc-templater repo](https://github.com/uncletammy/doc-templater)。

每一个.md文件在网站上都有自己的页面(也就是说所有的引用、概念、和框架文件), 并且应该都包含一个特殊的`<docmeta name="displayName">`标签，其`value`的属性指定为该页面的标题。这个将会影响到文档在在搜索引擎中的结果的显示，并且它也会作为显示名称显示在sailsjs.org网站上的导航栏菜单。比如：

```markdown
<docmeta name="displayName" value="Building Custom Homemade Puddings">
```

#### 我的修改什么时候可以显示在Sails的网站上？

当它们被合并到一个特殊的分支，对应于当前Sails稳定版本（比如0.12）的时候修改是立即生效的。我们无法合并那些直接发送到分支的pull请求--它的唯一目的是反映目前托管在sailsjs.org内容，并且内容只会在重新部署Sails网站之前才会合并。

如果你想要看到文档的改动是如何显示在sailsjs.org的话，你可以访问[preview.sailsjs.org](http://preview.sailsjs.org)。这个预览网站将会自己自动更新合并到master分支上的改动。


#### 我如何帮忙翻译这些文档呢？

帮助Sails工程的一个最好的方法就是自愿帮助翻译Sails文档，尤其如果你不是讲英语当做母语。如果你对上面的表中罗列的其他语言项目感兴趣的话，可以通过在fork项目啊中README中的步骤来联系对应翻译工程的维护者。

如果你的语言没有罗列在上面的表中，并且你对翻译工作感兴趣，那么遵循以下步骤：

+ Fork这个代码库(`balderdashy/sails-docs`)并改变你fork的名字为`sails-docs-{{IETF}}`， 其中 {{IETF}}是你的语言对应到[IETF语言标签](https://en.wikipedia.org/wiki/IETF_language_tag)中的一个。
+ 编辑README文件来概括目前你的翻译工作进度，并提供任何你觉得对其他阅读你的翻译文档的人的信息，并且让感兴趣的贡献者可以知道如何联系到你。
+ 编辑上面的表项以添加你fork的连接然后发送一个pull请求
+ 当你对于你的翻译工作的第一个版本比较满意的时候，开启一个issue然后我们文档小组的某些人将会乐于帮助你在Sails网站上预览这些内容，并让它在一个域名中显示(也许是你的域名，或者是sailsjs.org的子域名, 哪个是最好的选择呢？), 最后将其分享到Sails社区中的其他地方。


#### 还有别的我能帮上忙的吗？

更多关于贡献Sails文档的信息，可以参考[Contribution Guide](https://github.com/balderdashy/sails/blob/master/CONTRIBUTING.md).

#### 目前文档翻译进度

+ sails-docs-zh-cn/README.md
+ sails-docs-zh-cn/concepts/** (**代表该目录下所有文件已经翻译完毕)


#### 如何联系我？

鉴于英语水平有限，翻译过程中有不对的地方，大家可以完全修改它并提交。其他章节有兴趣的童鞋也可以参与其中。不胜感激。

联系方式： linguang661990@126.com
