## 代码覆盖率
另外一个测试你的代码的流行方法是[代码覆盖率](http://en.wikipedia.org/wiki/Code_coverage)。

你可以使用[mocha](http://mochajs.org/)和[istanbul](https://github.com/gotwarlost/istanbul)去检查你的代码并准备各种各样的覆盖率报告(HTML, Cobertura)，这些报告可以服务于一些持续集成测试工具比如jenkins。

为了测试你的代码并准备一段简单的HTML报告，可以运行下面的命令：

```bash
istanbul cover -x "**/config/**" _mocha -- --timeout 5000
istanbul report html
```

<docmeta name="displayName" value="Code Coverage">
