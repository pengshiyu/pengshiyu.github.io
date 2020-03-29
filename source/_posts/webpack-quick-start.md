---
title: Webpack快速入门
date: 2020-03-29 16:19:48
tags:
    - Webpack
categories:
    - 前端
---

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)

<!--more-->

## 一、webpack简单入门
1、webpack安装

```bash
# 安装webpack
npm init -y
npm install webpack webpack-cli --save-dev

# 查看版本
./node_modules/.bin/webpack -v
4.42.1
```

2、准备要打包的文件
src/hellowebpack.js
```js
export function hellowebpack() {
    return 'hellowebpack'
}
```

src/index.js
```js
import { hellowebpack } from './hellowebpack'

document.write(hellowebpack())
```

3、配置webpack.config.js
```js
'use strict'

const path = require('path');

module.exports = {
    // 打包入口
    entry: {
        index: './src/index.js',
    },

    // 指定输出地址及打包出来的文件名
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'index.js'
    },

    // 开发环境
    mode: 'production',
}
```

4、执行打包
```bash
./node_modules/.bin/webpack
# dist 文件夹下多出一个index.js文件
```

5、引用打包文件
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <script src="./index.js" type="text/javascript"></script>
</body>

</html>
```

6、配置打包命令
package.json
```json
 "scripts": {
    "build": "webpack"
  }
```

打包命令
```bash
npm run build
```

此时的项目结构为：
```
.
├── dist
│   ├── index.html
│   └── index.js
├── node_modules
├── package.json
├── src
│   ├── hellowebpack.js
│   └── index.js
└── webpack.config.js
```

## 二、loader
配置完之后都需要重新打包生效

1、babel解析es6
```bash
cnpm i @babel/core @babel/preset-env babel-loader -D
```

根目录新建.babelrc 并配置
```js
{
    "presets": [
        "@babel/preset-env",
    ]
}
```

配置 webpack.config.js, 添加module属性
```js
'use strict'
const path = require('path');

module.exports = {
    // 打包入口
    entry: {
        index: './src/index.js',
    },

    // 指定输出地址及打包出来的文件名
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'index.js'
    },

    module: {
        rules: [
            {
                test: /.js$/,
                use: 'babel-loader'
            }
    },

    // 开发环境
    mode: 'production',
}
```

babel编译效果
```js
// 编译前
document.write(() => hellowebpack())

// 编译后
document.write((function(){return"hellowebpack"}))
```

2、加载css文件
css-loader用于加载css文件并生成commonjs对象

style-loader用于将样式通过style标签插入到head中

```
npm i style-loader css-loader -D
```

配置 webpack.config.js
```js
{
    test: /.css$/,
    // 执行的时候是先加载css-loader，将css解析好后再将css传递给style-loader
    use: [
        'style-loader',
        'css-loader',
    ]
}
```

index.js中引入css文件
public.css
```css
.color-text {
    font-size: 20px;
    color: red
}
```

index.js
```js
import { hellowebpack } from './hellowebpack'
import "./public.css"

document.write(hellowebpack())
```

3、加载less
```bash
npm i less less-loader -D
```

配置 webpack.config.js
```js
{
    test: /.less$/,
    use: [
        'style-loader',
        'css-loader',
        'less-loader'
    ]
}

```

4、加载图片
```bash
# url-loader直接内置了file-loader
npm i url-loader -D
```

配置 webpack.config.js
```js
{
    test: /.(jpg|png|gif|jpeg)$/,
    use: [{
        loader:'url-loader',
        options: {
            // 单位是字节, 如果图片大小小于这个值，就会被打包为base64格式
            limit:160000,
            name: 'imgs/[name].[hash].[ext]'
        }
    }]
}
```

## 三、完整配置
目录结构
```
$ tree -L 2
.
├── dist
│   ├── imgs
│   ├── index.html
│   └── index.js
├── node_modules
├── src
│   ├── demo.png
│   ├── hellowebpack.js
│   ├── index.js
│   └── public.less
├── .babelrc
├── package.json
└── webpack.config.js
```
三个根目录下的配置文件
.babelrc
```js
{
    "presets": [
        "@babel/preset-env",
    ]
}
```
package.json
```json
{
  "name": "web-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack"
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
    "less": "^3.11.1",
    "less-loader": "^5.0.0",
    "style-loader": "^1.1.3",
    "url-loader": "^4.0.0",
    "webpack": "^4.42.1",
    "webpack-cli": "^3.3.11"
  }
}

```

webpack.config.js
```js
'use strict'
const path = require('path');

module.exports = {
    // 打包入口
    entry: {
        index: './src/index.js',
    },

    // 指定输出地址及打包出来的文件名
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'index.js'
    },

    module: {
        rules: [
            {
                test: /.js$/,
                use: 'babel-loader'
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

>参考
[一小时的时间，上手 Webpack](https://mp.weixin.qq.com/s/fDq_XC4LYuTO2us0Uin9rw)

