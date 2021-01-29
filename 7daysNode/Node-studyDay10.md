---
title: Node-studyDay10
date: 2020-12-24 14:21:04
tags: Node
categories: Node
description: Express学习
---

一、简介

Express 是一种保持最低程度规模的灵活Node.js Web应用程序框架，为Web和移动应用程序提供一组强大的功能。Express框架是后台的Node框架。

二、性能

Express 提供精简的基本Web应用程序功能，而不会隐藏您了解和青睐的Node.js功能。Express不对Node.js已有的特性进行二次抽象，我们只是在它之上扩展了Web应用所需的功能。丰富的HTTP工具以及来自Content框架的中间件随取随用，创建强健、友好的API变得快速又简单。

三、起步教程

###### 配置package.json，使用npm安装依赖

- 1、新建一个项目目录，在项目目录下，打开cmd,快速初始化一个项目依赖文件package.json

```cmd
npm init -y
```

- 2、在项目目录中打开cmd安装Express

```cmd
npm install express --save
```

用--save安装的模块会添加到package.json文件中的dependencies里，以后运行项目目录中的npm i将自动安装依赖项列表中的模块。

- 3、在项目目录下创建app.js文件，文件中写入以下代码

```js
let express = require('express')
let app = express()

app.get('/', function (req, res) {
  res.send('Hello World!')
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```

- 4、在项目目录中打开cmd，运行以下命令

```cmd
node app.js
```

此时在浏览器中访问`http://localhost:3000`就能看到服务器成功开启。而访问其他的路径则会以404 Not Found响应。

###### 使用Express应用程序生成器

- 1、首先打开cmd安装一下express

```cmd
npm i express-generator -g
```

- 2、使用以下命令创建我们的第一个Express应用程序

```cmd
express --view=pug Express-demo
```

- 3、根据提示依次执行以下命令，进入项目目录中并且安装依赖

```cmd
cd Express-demo
```

```cmd
npm install
```

- 4、启动程序
  在MasOS或Linux上，采用以下命令运行此应用程序

```cmd
$ DEBUG=Expree-demo:* npm start
```

在Windows上，使用以下命令：（|符号是`或者`的意思）

```cmd
set DEBUG=Expree-demo:* & npm start | npm start			{2个命令可选}
```

然后在浏览器中输入 http://localhost:3000/ 访问此应用程序。

###### Express初次感知

`app.js`

```js
// 0. 安装
// 1. 引包
var express = require('express')

// 2. 创建你服务器应用程序
//    也就是原来的 http.createServer
var app = express()


// 在 Express 中开放资源就是一个 API 的事儿
// 公开指定目录
// 只要这样做了，你就可以直接通过 /public/xx 的方式访问 public 目录中的所有资源了
app.use('/public/', express.static('./public/')) // 访问http://localhost:3000/public/js/main.js看看
app.use('/static/', express.static('./static/')) 
app.use('/node_modules/', express.static('./node_modules/'))

// 模板引擎，在 Express 也是一个 API 的事儿

// 得到路径
// 一个一个的判断
// 以前的代码很丑

app.get('/about', function (req, res) {
  // 在 Express 中可以直接 req.query 来获取查询字符串参数
  console.log(req.query)
  res.send('你好，我是 Express!')
})

app.get('/pinglun', function (req, res) {
  // req.query
  // 在 Express 中使用模板引擎有更好的方式：res.render('文件名， {模板对象})
  // 可以自己尝试去看 art-template 官方文档：如何让 art-template 结合 Express 来使用
})

// 当服务器收到 get 请求 / 的时候，执行回调处理函数
app.get('/', function (req, res) {
  res.send(`
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
<body>
  <h1>hello Express！你好</h1>
</body>
</html>
`)
})

// 相当于 server.listen
app.listen(3000, function () {
  console.log('app is running at port 3000.')
})

```

![express-demob0d5766135f31de4.png](https://cdn.longdoer.com/2020/12/24/express-demob0d5766135f31de4.png)

###### 文件操作路径和模块标识路径问题

```js
var fs = require('fs')

// 我们使用的所有文件操作的 API 都是异步的
// 就像你的 ajax 请求一样
// 文件操作中的相对路径可以省略 ./
fs.readFile('data/a.txt', function (err, data) {
  if (err) {
    return console.log('读取失败')
  }
  console.log(data.toString())
})
```

在模块加载中，相对路径中的` ./ `不能省略

```js
require('data/foo.js')
//Error: Cannot find module 'data/foo.js'

require('./data/foo.js')('hello')
//hello
//hello Node!
```

在文件操作的相对路径中

> `./data/a.txt `  相对于当前目录
> 	  ` data/a.txt`      相对于当前目录
>   	 `/data/a.txt `   绝对路径，当前文件模块所处磁盘根目录
>   	` C:/xx/xx...`    绝对路径

```js
//输出 hello Node!
fs.readFile('./data/a.txt', function (err, data) {
  if (err) {
    console.log(err)
    return console.log('读取失败')
  }
  console.log(data.toString())
})

// 这里如果忽略了 . 则也是磁盘根目录
//require('/data/foo.js')
//Error: Cannot find module 'data/foo.js'
```

![node_roadMapTipsc7367aaa6dfdd8e3.png](https://cdn.longdoer.com/2020/12/24/node_roadMapTipsc7367aaa6dfdd8e3.png)

## nodemon

nodemon是一种工具，可以自动检测到目录中的文件更改时通过重新启动应用程序来调试基于node.js的应用程序。

##### 安装

```cpp
npm install -g nodemon
//或
npm install --save-dev nodemon
```

##### 使用

```cpp
nodemon   ./main.js // 启动node服务
```

```cpp
nodemon ./main.js localhost 6677 // 在本地6677端口启动node服务
```

```bash
"start": "ts-node -r tsconfig-paths/register nodemon src/main.ts",
```

##### 延迟重启

```css
nodemon -delay10 main.js

nodemon --delay 2.5 server.js

nodemon --delay 2500ms server.js
```

这个就类似于js函数中的函数节流,只在最后一次更改的文件往后延迟重启.避免了短时间多次重启的局面.

##### 配置文件

nodemon支持本地和全局配置文件。这些通常是命名的`nodemon.json`，可以位于当前工作目录或主目录中。可以使用该`--config <file>`选项指定备用本地配置文件。

```json
{
  "verbose": true,
  "ignore": ["*.test.js", "fixtures/*"],
  "execMap": {
    "rb": "ruby",
    "pde": "processing --sketch={{pwd}} --run"
  }
}
```

##### Doc

- [nodemon](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fnodemon)

