title: Yeoman: Web 应用开发流程与工具
speaker: perry 王志佩
url: https://github.com/Perrywzp/perryPPTs/blob/master/yeoman/main.md
transition: slide3
theme: moon
usemathjax: yes

[slide]
# Yeoman:让你快速、优雅的CODING
<small>演讲者：王志佩</small>

[slide ]
##yeoman是什么？
* <span class="blue">yeoman</span> 可以帮助你快速启动一个新项目，提供最好的方案和工具来帮助你保持生产力。

* 这样做，我们提供了一个<span class="blue">generator</span>构建系统。这个<span class="blue">generator</span>是一个基础的插件，它可以使用 `yo` 命令去构建完成一个项目或者其它一些有用的部分。

* 通过官方的 Generators, 我们将推进 "Yeoman 的工作流"。这是一个强大的和稳定的工作流客户端栈，包括工具和框架，它可以帮助开发人员快速构建漂亮的Web应用程序。我们细心的提供一切需要并很头痛的项目起步设置，原先你需要手动设置。

[slide ]
##yeoman 是由以上三种类型的工具组成的。
----

* 脚手架工具 yo
* 构建工具 gulp,grunt,webpack etc
* 包管理工具 npm,bower

----
今天主要介绍的是webpack的配置，和yo的使用。

[slide ]
## YO 构建一个webApp
* 在安装了nodejs环境以后，安装yeoman工具集,
```
npm install --global yo
```
* 安装一个generator
```
npm install --global generator-fountain-webapp
```
为这个generator安装它的Node依赖包

* 通过Yeoman菜单访问你的generators
再次运行yo 来查看你的generators
```
yo
```
通过方向键可以选择你要的generator，敲回车键便可运行这个generator了；

* 直接的使用generators
```
yo fountain-webapp
```

[slide]
## 配置你的generator
### 很多generators是会提供可选的设置自定义您的应用程序与常见的开发库, 一般包含以下选项:
* 框架(React, angular2, vue)
* 模块管理(webpack),构建工具(grunt, gulp)
* javascript预处理器(Babel, TypeScript,或可不用)
* css预处理器(SASS, LESS, STYLUS, 或可不用)
* 三个示例app（a landing page, hello world, TODOMVC）

[slide]
## generator生成的项目结构如下图
----
<div class="columns-2">
	<img src="/imgs/bmps/reactWebappYo.png" height="850">
</div>
<style>
 .perry-columns-2 {
 	max-width: 800px !important;
 }
</style>


[slide]
## 目录内容描述
* src 应用程序的父目录
	* app react+redux代码
	* index.html 基本的html文件
	* index.js app的入口文件
* conf 第三方工具给我们的配置文件的目录
* gulp_tasks 和gulpfile.js 构建任务文件
* .babelrc,package.json,和node_modules 配置和必要的依赖
* .gitattributes和.gitignore git配置文件
[slide]
## webpack
<span class="red">安装:</span>npm install -g webpack

[slide]
## Webpack 的特点

* <span class="label label-success">代码拆分</span>
Webpack 有两种组织模块依赖的方式，同步和异步。异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。

* <span class="label label-success">Loader</span>
Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

* <span class="label label-success">智能解析</span>
Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 require("./templates/" + name + ".jade")。

* <span class="label label-success">插件系统</span>
Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求。

* <span class="label label-success">快速运行</span>
Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。

[slide]
##sever.js
<pre>
	<code class="javascript">
		'use strict';
		require('core-js/fn/object/assign');
		const webpack = require('webpack');
		const WebpackDevServer = require('webpack-dev-server');
		const config = require('./webpack.config');
		const open = require('open');

		new WebpackDevServer(webpack(config), config.devServer)
		.listen(config.port, 'localhost', (err) => {
		  if (err) {
			console.log(err);
		  }
		  console.log('Listening at localhost:' + config.port);
		  console.log('Opening your system browser...');
		  open('http://localhost:' + config.port + '/webpack-dev-server/');
		});
	</code>
</pre>
[slide]
##webpack.config.js
<pre>
	<code class="javascript">
		'use strict';

		const path = require('path');
		const args = require('minimist')(process.argv.slice(2));

		// List of allowed environments
		const allowedEnvs = ['dev', 'dist', 'test'];
		console.log(process.argv);
		console.log(args);
		// Set the correct environment
		let env;
		if (args._.length > 0 && args._.indexOf('start') !== -1) {
		  env = 'test';
		} else if (args.env) {
		  env = args.env;
		} else {
		  env = 'dev';
		}
		process.env.REACT_WEBPACK_ENV = env;

		/**
		 * Build the webpack configuration
		 * @param  {String} wantedEnv The wanted environment
		 * @return {Object} Webpack config
		 */
		function buildConfig(wantedEnv) {
		  let isValid = wantedEnv && wantedEnv.length > 0 && allowedEnvs.indexOf(wantedEnv) !== -1;
		  let validEnv = isValid ? wantedEnv : 'dev';
		  let config = require(path.join(__dirname, 'cfg/' + validEnv));
		  return config;
		}

		module.exports = buildConfig(env);
	</code>
</pre>

[slide]
<pre>
	<code>
		// base.js
		'use strict';
        let path = require('path');
        let defaultSettings = require('./defaults');
		let additionalPaths = [];
        module.exports = {
          additionalPaths: additionalPaths,
          port: defaultSettings.port,
          debug: true,
          devtool: 'eval',
          output: {
            path: path.join(__dirname, '/../dist/assets'),
            filename: 'app.js',
            publicPath: defaultSettings.publicPath
          },
          devServer: {
            contentBase: './src/',
            historyApiFallback: true,
            hot: true,
            port: defaultSettings.port,
            publicPath: defaultSettings.publicPath,
            noInfo: false
          },
          resolve: {
            extensions: ['', '.js', '.jsx'],
            alias: {
              actions: `${defaultSettings.srcPath}/actions/`,
              components: `${defaultSettings.srcPath}/components/`,
              sources: `${defaultSettings.srcPath}/sources/`,
              stores: `${defaultSettings.srcPath}/stores/`,
              styles: `${defaultSettings.srcPath}/styles/`,
              config: `${defaultSettings.srcPath}/config/` + process.env.REACT_WEBPACK_ENV
            }
          },
          module: {}
        };
	</code>
</pre>

[slide]
##default.js
<pre>
	<code>
        'use strict';
        const path = require('path');
        const srcPath = path.join(__dirname, '/../src');
        const dfltPort = 8000;
        function getDefaultModules() {
            return {
                preLoaders: [{ test: /\.(js|jsx)$/, include: srcPath, loader: 'eslint-loader' }],
                loaders: [
                	{ test: /\.css$/, loader: 'style-loader!css-loader' },
                    { test: /\.sass/, loader: 'style-loader!css-loader!sass-loader?outputStyle=expanded&indentedSyntax' },
                    { test: /\.scss/, loader: 'style-loader!css-loader!sass-loader?outputStyle=expanded' },
                    { test: /\.less/, loader: 'style-loader!css-loader!less-loader' },
                    { test: /\.styl/, loader: 'style-loader!css-loader!stylus-loader' },
                    { test: /\.(png|jpg|gif|woff|woff2|eot|ttf|svg)$/, loader: 'url-loader?limit=8192' },
                    { test: /\.json/, loader: 'json-loader' },
                    { test: /\.(mp4|ogg|svg)$/, loader: 'file-loader' }]
            };
        }
        module.exports = {
            srcPath: srcPath,
            publicPath: '/assets/',
            port: dfltPort,
            getDefaultModules: getDefaultModules
        };
	</code>
</pre>

[slide]
##dev.js
<pre>
	<code>
		'use strict';
        let path = require('path');
        let webpack = require('webpack');
        let baseConfig = require('./base');
        let defaultSettings = require('./defaults');
        let BowerWebpackPlugin = require('bower-webpack-plugin');
        let config = Object.assign({}, baseConfig, {
          entry: [
            'webpack-dev-server/client?http://127.0.0.1:' + defaultSettings.port,
            'webpack/hot/only-dev-server',
            './src/index'
          ],
          cache: true,
          devtool: 'eval-source-map',
          plugins: [
            new webpack.HotModuleReplacementPlugin(),
            new webpack.NoErrorsPlugin(),
            new BowerWebpackPlugin({
              searchResolveModulesDirectories: false
            })
          ],
          module: defaultSettings.getDefaultModules()
        });
        config.module.loaders.push({
          test: /\.(js|jsx)$/,
          loader: 'react-hot!babel-loader',
          include: [].concat(
            config.additionalPaths,
            [ path.join(__dirname, '/../src') ]
          )
        });
        module.exports = config;
	</code>
</pre>

[slide]
## Yeoman特性总结
----
* 快速创建骨架应用程序——使用可自定义的模板（例如：HTML5、Boilerplate、Twitter Bootstrap等）、AMD（通过RequireJS）以及其他工具轻松地创建新项目的骨架。
* 自动编译CoffeeScrip和Compass——在做出变更的时候，Yeoman的LiveReload监视进程会自动编译源文件，并刷新浏览器，而不需要你手动执行。
* 自动完善你的脚本——所有脚本都会自动针对jshint（软件开发中的静态代码分析工具，用于检查JavaScript源代码是否符合编码规范）运行，从而确保它们遵循语言的最佳实践。
* 内建的预览服务器——你不需要启动自己的HTTP服务器。内建的服务器用一条命令就可以启动
* 非常棒的图像优化——Yeoman使用OptPNG和JPEGTran对所有图像做了优化，从而你的用户可以花费更少时间下载资源，有更多时间来使用你的应用程序。

[slide]
----
* 生成AppCache清单——Yeoman会为你生成应用程序缓存的清单，你只需要构建项目就好
* 杀手级”的构建过程——你所做的工作不仅被精简到最少，让你更加专注，而且Yeoman还会优化所有图像文件和HTML文件、编译你的CoffeeScript和Compass文件、生成应用程序的缓存清单，如果你使用AMD，那么它还会通过r.js来传递这些模块。这会为你节省大量工作
* 集成的包管理——Yeoman让你可以通过命令行（例如，yeoman搜索查询）轻松地查找新的包，安装并保持更新，而不需要你打开浏览器
* 对ES6模块语法的支持——你可以使用最新的ECMAScript 6模块语法来编写模块。这还是一种实验性的特性，它会被转换成eS5，从而你可以在所有流行的浏览器中使用编写的代码
* PhantomJS单元测试——你可以通过PhantomJS轻松地运行单元测试。当你创建新的应用程序的时候，它还会为你自动创建测试内容的骨架
