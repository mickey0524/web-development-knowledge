* webpack插件大全

	https://webpack.js.org/plugins/

* webpack优秀中文文章
	
	https://github.com/mickey0524/awesome-webpack-cn

* webpack与flexbile.js配合实现自适应

	webpack在处理样式打包的时候在less,sass等loader之前，在style-loader，css-loader之后，使用postcss-loader，配置postcss-pxtorem，就能将按照设计稿设计的px属性转为自适应的rem属性 https://github.com/cuth/postcss-pxtorem	

* 实现一个webpack📦工具的基本思路

    把页面逻辑当作一个整体，通过一个给定的入口文件，打包工具从这个文件开始，找到所有的依赖文件，进行打包、编译、压缩，最终输出一个浏览器可识别的js文件

    一个模块打包工具，第一步会从入口文件开始，对其进行依赖分析，第二步对其所有依赖再次递归进行代码分析，第三步构建出模块的依赖图集，最后一步根据依赖图集使用CommonJS规范构建出最终的代码
    
    [参考资料，parcel核心作者写的demo--minipack](https://github.com/ronami/minipack/blob/master/src/minipack.js)
