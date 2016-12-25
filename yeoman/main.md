title: Yeoman: Web 应用开发流程与工具
speaker: perry 王志佩
url: https://github.com/Perrywzp/perryPPTs/blob/master/yeoman/main.md
transition: slide3
theme: green
usemathjax: yes

[slide style="background-image:url('/imgs/moment/canada.jpg')"]
# Yeoman:Web 应用开发流程与工具
<small>演讲者：王志佩</small>


[slide style="background-image:url('/imgs/moment/archLake.jpg')"]
## Yeoman特性总结
----
* 快速创建骨架应用程序——使用可自定义的模板（例如：HTML5、Boilerplate、Twitter Bootstrap等）、AMD（通过RequireJS）以及其他工具轻松地创建新项目的骨架。
* 自动编译CoffeeScrip和Compass——在做出变更的时候，Yeoman的LiveReload监视进程会自动编译源文件，并刷新浏览器，而不需要你手动执行。
* 自动完善你的脚本——所有脚本都会自动针对jshint（软件开发中的静态代码分析工具，用于检查JavaScript源代码是否符合编码规范）运行，从而确保它们遵循语言的最佳实践。
* 内建的预览服务器——你不需要启动自己的HTTP服务器。内建的服务器用一条命令就可以启动
* 非常棒的图像优化——Yeoman使用OptPNG和JPEGTran对所有图像做了优化，从而你的用户可以花费更少时间下载资源，有更多时间来使用你的应用程序。

[slide style="background-image:url('/imgs/moment/england.jpg')"]
----
* 生成AppCache清单——Yeoman会为你生成应用程序缓存的清单，你只需要构建项目就好
* 杀手级”的构建过程——你所做的工作不仅被精简到最少，让你更加专注，而且Yeoman还会优化所有图像文件和HTML文件、编译你的CoffeeScript和Compass文件、生成应用程序的缓存清单，如果你使用AMD，那么它还会通过r.js来传递这些模块。这会为你节省大量工作
* 集成的包管理——Yeoman让你可以通过命令行（例如，yeoman搜索查询）轻松地查找新的包，安装并保持更新，而不需要你打开浏览器
* 对ES6模块语法的支持——你可以使用最新的ECMAScript 6模块语法来编写模块。这还是一种实验性的特性，它会被转换成eS5，从而你可以在所有流行的浏览器中使用编写的代码
* PhantomJS单元测试——你可以通过PhantomJS轻松地运行单元测试。当你创建新的应用程序的时候，它还会为你自动创建测试内容的骨架
