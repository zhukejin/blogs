使用 r.js 压缩Javascript代码
--

在使用`RequireJs`优化了代码后，自然的就看到了 `RequireJs`提供的 `r.js` 代码压缩

准备工具：

- 内含`Javascript`的代码一套（TAT）
- `NodeJs`运行环境(V8)

首先值得说明的是 r.js 压缩无论代码是否遵循 AMD 规范都可进行压缩，首先前往`RequireJs`官网下载最新版的 r.js [传送门](http://requirejs.org/docs/release/2.1.4/r.js) 点不开的来`github`下载 [传送门](https://github.com/zhukejin1223/Blog_demo/blob/master/r.js)

如果手头没有 `NodeJs`的话,[点击这里搞一个](http://nodejs.org/)

一切准备好后，先来看一下压缩前的代码结构

- DemoIndex ----------------------------- 存放资源文件的文件夹
	- css
	- js	
		- app
		- lib
		- app.js ------------------------ RequireJs的入口文件
		- require.js -------------------- RequireJs
	- php
- build.js ------------------------------------ r.js 配置文件
- r.js ---------------------------------------- r.js
- index.html

这里 `r.js` 与其配置文件在项目根目录放置，来看下配置文件 `build.js` : 

	({
		//需要打包的文件夹，我这里是一个整个项目文件夹，也可以是单独模块
	    appDir: "./",

		//这里和RequireJs.config 保持一致，为了方便r.js拼合依赖关系
	    baseUrl: "DemoIndex/js/lib",

		//这个路径是代码打包后存放的路径
	    dir: "../build",

		//这里和RequireJs.config 保持一致，为了方便r.js拼合依赖关系
	    paths: {
	        app: '../app',
	        jquery : 'jquery-1.8.0.min'
	    }
	})

接下来，打开`Node`的运行环境，进入到项目所在目录，比如我是 `E://workspace/`

然后执行：
	
	node r.js -o build.js

然后稍等片刻， over. 这时候再看代码结果 (../build 这个路径中的)，整个项目代码就全被压缩过一遍了，size 也小了很多..

110 kb -> 75 kb (压缩率还是很高的)。

