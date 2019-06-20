---
title: "webpack简单打包"  
date: 2018-12-05
comments: true  
tags: 
	- npm
	- webpack
---


### webpack安装
安装为开发依赖：`npm install webpack –save-dev`  
>简写`npm -i webpack -D`


### 项目配置
<!-- more -->
1. 在项目文件夹下创建**webpack.config.js**文件  
2. 最简配置
	```bash  
	const path = require('path')　　 //引入node路径模块  
	module.exports = {  
	　　entry: './入口文件.js',		//输入项  
	　　output: {  
	　　　path: path.resolve(__dirname,'dist'),//获取路径为当前文件夹下  
	　　　filename: 'main.js'　　//打包输出为此文件  
	　　}  
	}
	```
	>配置好之后就可以进行最简单的模块化打包了  
  
3. 在npm配置的scripts中设置打包指令`"dev":"webpack"`  
4. 命令行中输入`npm run dev`开始打包  
  

* ### 插件  
　　webpack插件一般结尾为webpack-plugin，例如较为常用的`html-webpack-plugin`，`clean-webpack-plugin`等；  
　　通过npm将插件下载之后，引入`webpack.config.js`中，格式为：	```bash  
var htmlWebpackPlugin = require("html-webpack-plugin")//插件作用为通过特定配置输出html文件  
module.exports = {  
...  
　　output:{},  
　　plugins:[  
　　　　new htmlWebpackPlugin()//可以在内部以对象格式添加配置{filename:'输出文件名',template:'文件模板'}  
	]  
}
	```
>[关于html-webpack-plugin的详细配置](https://doc.webpack-china.org/plugins/html-webpack-plugin/)  

* ### 预处理模块(loader)*  
```bash
module.exports = {  
...  
　　output:{},  
　　plugins:[],
　　module:｛
　　　　rules:[{
　　　　　　test:/\.js$/,//匹配规则，此正则表示匹配所有js文件
　　　　　　use:[{
         loader:'bable-loader'，//匹配成功后使用的编译器,需要npm下载
         options:{
         　　各种设置
         }
       }]
　　　　}]
　　｝
}
```

* ### 测试服务器(devserver)  
npm安装:`npm i -D webpack-dev-server`  
作用：更改页面时自动替换页面内容（热替换）  
```bash
module.exports = {  
...  
　　module:｛},
　　devServer:{
　　　　open:true,//自动打开浏览器测试
　　　　port:8080,//设置端口
　　　　
　　}
}
```

至此关于webpack配置文件的基本结构已经大致介绍了一遍，之后会整理各种配置的详细信息，以及常用插件模块，也可以自行查看[webpack官方文档](https://doc.webpack-china.org/concepts/)

---
![](https://cl.ly/1T1D0Y1x0h0R/j8.gif)