---
title: Webpack插件使用及热更新打包
date: 2020-03-29 17:40:17
tags:
    - Webpack
categories:
    - 前端
---

1、HtmlWebpackPlugin 自动生成基本的 html 页面
2、开启文件监听
3、webpack-dev-server热更新

<!--more-->

安装依赖
```bash
cnpm i html-webpack-plugin webpack-dev-server -D
```

配置文件
1、.babelrc
```clike
{
    "presets": [
        "@babel/preset-env",
    ]
}
```

2、package.json
```json
{
  "name": "web-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "watch": "webpack --watch",
    "dev": "webpack-dev-server --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.9.0",
    "@babel/preset-env": "^7.9.0",
    "babel-loader": "^8.1.0",
    "css-loader": "^3.4.2",
    "file-loader": "^6.0.0",
    "html-webpack-plugin": "^4.0.3",
    "less": "^3.11.1",
    "less-loader": "^5.0.0",
    "style-loader": "^1.1.3",
    "url-loader": "^4.0.0",
    "webpack": "^4.42.1",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

3、webpack.config.js
```js
'use strict'
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require('webpack');

module.exports = {
    // 文件监听
    watch: true,

    watchOptions: {
        //默认为空，设置不监听的文件或者文件夹，支持正则匹配
        ignored: /node_modules/,
        //监听到变化发生后会等300ms再去执行，默认300ms
        aggregateTimeout: 300,
        //设置轮询文件是否变化时间，默认每秒问1000次
        poll: 1000
    },

    // 打包入口
    entry: {
        index: './src/index.js',
    },

    // 指定输出地址及打包出来的文件名
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'index.js'
    },

    plugins: [
        // 自动生成基本的 html 页面
        new HtmlWebpackPlugin({
            title: 'leaningwebpack',        // 文件的标题
            filename: 'webpack-index.html', // 文件名
            favicon: 'src/demo.png'          // 网页图标
        }),

        // 热更新
        new webpack.HotModuleReplacementPlugin()
    ],

    devServer: {
        contentBase: './dist',
        hot: true
    },

    module: {
        rules: [
            {
                test: /.js$/,
                use: 'babel-loader',
                // 忽略依赖包
                exclude: /node_modules/
            },
            {
                test: /.css$/,
                // 执行的时候是先加载css-loader，将css解析好后再将css传递给style-loader
                use: [
                    'style-loader',
                    'css-loader',
                ]
            },
            {
                test: /.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /.(jpg|png|gif|jpeg)$/,
                use: [{
                    loader: 'url-loader',
                    options: {
                        // 如果图片大小小于这个值，就会被打包为base64格式
                        limit: 160000,
                        name: 'imgs/[name].[hash].[ext]'
                    }
                }]
            }
        ]
    },

    // 开发环境
    mode: 'production',
}
```

> 参考
[Webpack 实战入门系列（二）：插件使用及热更新打包](https://mp.weixin.qq.com/s/ggCFFYfpoPPBj7geOJv4nQ)
