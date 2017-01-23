# Webpack

## 开始
`npm init` 创建`package.json`

## 安装Webpack
`npm i webpack --save-dev`

## 目录结构

* /app
	- index.js
	- component.js
* /build
	- bundle.js (自动创建)
	- index.html
* package.json
* webpack.config.js

## 设置Webpack

webpack.config.js:

```js
var path = require('path');

var config = {
	entry: [
		'webpack/hot/dev-server',
		'webpack-dev-server/client?http://localhost:8080',
		path.resolve(__dirname, 'src/index.js'),
		],
	output: {
		path: path.resolve(__dirname, 'build'),
		filename: 'bundle.js',
	},
	module: {
		loaders: [
			{
				test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
				exclude: /node_modules/,
				loader: 'babel',
				query: {
					presets: ['es2015', 'react']
				}
			},
			{
				test: /\.css$/,
				loader: 'style!css'
			},
			{
				test: /\.(png|jpg|jpeg|gif|woff)$/,
				loader: 'url?limit=8192'
			},
			{
				test: /\.svg$/,
				loader: 'file'
			}
		]
	}
};

module.exports = config;
```

## 安装基础依赖包

使用 `npm`

```shell
npm i react react-dom --save

npm i --save-dev babel-core babel-loader css-loader file-loader style-loader url-loader webpack
```

使用 `yarn`

```shell
yarn add react react-dom react-router

yarn add webpack webpack-dev-server -D
yarn add babel-core babel-loader babel-preset-es2015 bable-preset-react -D
yarn add css-loader file-loader url-loader -D
yarn add detect-port react-dev-utils -D
```