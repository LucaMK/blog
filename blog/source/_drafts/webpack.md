---
title: 简单聊一聊 webpack
tags: webpack js
---

## summary 摘要

首先本文简单介绍webpack, 并使用webpack4 实现 vue 单页项目的构建。

目标: 
1. webpack简介
2. 使用webpack搭建 vue单页项目的构建。

## webpack简单介绍

webpack 是一个JavaScript应用程序的 <bold>静态模块打包工具</bold>。 详细介绍可以查看官网 [中文](https://webpack.docschina.org/concepts/), [英文](https://webpack.js.org/concepts)。

打包工具webpack使得前端模块化开发更加方便，核心部分:
* [入口(entry)](https://webpack.docschina.org/concepts/#%E5%85%A5%E5%8F%A3-entry-)
* [输入(output)](https://webpack.docschina.org/concepts/#%E8%BE%93%E5%87%BA-output-)
* [loader](https://webpack.docschina.org/concepts/#loader)
* [插件(plugin)](https://webpack.docschina.org/concepts/#%E6%8F%92%E4%BB%B6-plugin-)
* [模式(mode)](https://webpack.docschina.org/concepts/#%E6%A8%A1%E5%BC%8F-mode-)
* [浏览器兼容性(browser compatibility)](https://webpack.docschina.org/concepts/#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9%E6%80%A7-browser-compatibility-)

以上内容可点击查看官网查详细介绍, 所有使用webpack 打包的项目都是包含上面几个部分, 这也体现出webpack本身就是模块化的。

## 简单实现vue单页项目的搭建:

创建项目文件夹，名字看个人喜好，此处使用webpack_vue2.6_demo

1、初始化项目, 安装webpack 和 vue 

```base
>npm init 

>npm install webpack --save-dev

>npm install webpack-cli --save-dev

>npm install vue --save

>npm install vue-router --save
```

2、创建模板文件 App.vue, 入口文件 main.js, 路由配置文件 router.js, HTML模板index.html,webpack配置文件 webpack.config.js:

本文档安装的依赖版本为 vue: 2.6.10 ; vue-router: 3.0.6; webpack: 4.31.0

创建后目录:
![](/images/webpack_vue2.6_folder.png)

```bash
//main.js
import Vue from 'vue'
import App from 'App'
import vueRouter from 'router'

new Vue({
	el: '#app',
	vueRouter,
	render: h => h(App)
})

//App.vue
<template>
	<div class="container">
		<h1>app title hi!</h1>
		<router-view></router-view>
	</div>
</template>

<script>
export default {
	name: 'App'
}
</script>

<style>
.container{
	width: 100%;
	display: flex;
	justify-content: center;
}
</style>

//helloworld_page.vue
<template>
	<div class="h-container">
		<img src="../../static/logo.png" alt="logo">
		<h1>this is helloworld page</h1>
		<h2>{{msg}}</h2>
	</div>
</template>

<script>
export default {
	name:'helloworld',
	data() {
		return {
			msg: 'this is hello msg'
		}
	}
}
</script>

<style lang="sass" scoped>
.h-container 
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		img
			width: 200px;
			height: auto;
		
		h1
			color: red;
		
		h2
			color: blue;
</style>

//router.js
import router from 'vue-router'
import HelloWorld from './pages/HelloWorld_page'

const route = new router({
	routers:[
		{
			path: '/',
			name: 'home_page',
			component: HelloWorld
		}
	]
})

```
##编写配置文件 webpack.config.js

```bash
// node.js 核心模块
var path = require('path');
var {VueLoaderPlugin} = require('vue-loader');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
	// 模式 暂时使用production
	mode: 'production',
	// 入口文件
	entry: {
		main: './src/main.js'
	},
	// 输出文件路径 和 名称
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: "[name].[hash].js"
	},
	// 添加模块需要使用的loader:
	module: {
		rules: [
			{
				// vue-loader 对.vue 文件进行处理 编写之前 npm i vue-loader vue-template-compiler --save-dev 进行安装
				// vue-template-compiler 独立安装,原因是可单独指定其版本, 不同版本vue 会对应不同版本的vue-template-compiler
				// 这样 vue-loader 可以生成兼容运行时的代码
				test: /\.vue$/,
				use: ['vue-loader']
			},
			{// css-loader 对css 文件处理 编写之前 npm i vue-style-loader css-loader --save-dev 
				test: /\.css$/,
				use: [
					'vue-style-loader',
					'css-loader'
				]
			},
			{// sass-loader 此处使用sass 预编译器 npm i sass-loader node-sass --save-dev
				test: /\.sass$/,
				use: [
					'vue-style-loader',
					'css-loader',
					{
						loader: 'sass-loader',
						options: {
							// 注意使用 lang = "sass" 时 需要提娜姬 indentedSyntax:true
							indentedSyntax: true
						}
					}
				]
			},
			{// 处理资源路径 , 此处选用 file-loader ,也可以选用 url-loader 如果需要将较小图片转为base64 可以选用url-loader (注意: 记得设置 limit 参数)
				test: /\.(png|jpg|gif)$/,
				use: [
					{
						loader: 'file-loader',
						options: {
							outputPath: 'images'
						}
					}
				]
			}
		]
	},
	// 添加默认解析后缀
	resolve: {
		extensions: ['.js', '.css', '.vue']
	},
	plugins: [
		//这个插件是必须的！ 它的职责是将你定义过的其它规则复制并应用到 .vue 文件里相应语言的块。例如，如果你有一条匹配 /\.js$/ 的规则，那么它会应用到 .vue 文件里的 <script> 块。
		new VueLoaderPlugin(),
		// 安装html-webpack-plugin  添加模板 index.html 
		new HtmlWebpackPlugin({
			filename: 'index.html',
			template: 'index.html',
			inject: true
		})
	]
}
```

在package.json 中 script 脚本模块 添加语句

```bash
"scripts": {
	"build": "webpack"
}

// 然后 执行 npm run build, 如果一切正常就会发现项目文件夹下 出现 dist 目录 这就是打包编译后的文件
```
当然现在直接访问 index.html 是没有页面渲染的，如果你有服务器可以放到服务器上即可出现之前编写的内容, 没有也没事，本地开启下nginx 服务代理指向当前dist目录 既可以在本地浏览器中 通过设置的 url 进行访问

当然 正常开发项目不是这样的, 会有一个开发模式 此处使用到 webpack-dev-server 简称[devServer](https://www.webpackjs.com/configuration/dev-server/)

## 开发模式 配置编写 

在项目根目录创建webpack.dev.conf.js

把上面写的 打包配置稍微更改即刻
