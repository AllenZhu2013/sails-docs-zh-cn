# 使用`.sailsrc`文件
除了配置你的app的其他方法，从Sails v0.10开始，你现在可以在`.sailsrc`文件中指定专用的配置给一个或者多个app(感谢Dominic Tarr的优秀模块-[rc](https://github.com/dominictarr/rc))。`rc`文件是配置命令行或者应用配置到所有在你的电脑上运行的Sails app中最有用的。

当Sails CLI运行一条命令的时候，它会在当前目录中和你的home目录中(也就是`~/.sailsrc`)(每个新生成的Sails app都会产生一个样板文件`.sailsrc`)查找`.sailsrc`文件(以JSON或者.ini格式)。然后它会将它们合并到已有的那些配置中。

> 实际上，Sails查找.sailsrc文件在一些其他地方(具体看rc conventions的配置)。你可以把一个.sailsrc放在那些路径的任意一个地方。即便如此，放置全局.sailsrc文件的最好的地方便是你的home目录(也就是~/.sailsrc)。




<docmeta name="displayName" value="Using `.sailsrc` Files">

