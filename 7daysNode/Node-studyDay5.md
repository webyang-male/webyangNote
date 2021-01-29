---
title: Node-studyDay5
date: 2020-12-23 11:29:49
tags: Node
categories: Node
description: art-template简单理使用
---

# 介绍

art-template 是一个简约、超快的模板引擎。

它采用作用域预声明的技术来优化模板渲染速度，从而获得接近 JavaScript 极限的运行性能，并且同时支持 NodeJS 和浏览器。[在线速度测试](http://aui.github.io/art-template/rendering-test/)。

## 特性

1. 拥有接近 JavaScript 渲染极限的的性能
2. 调试友好：语法、运行时错误日志精确到模板所在行；支持在模板文件上打断点（Webpack Loader）
3. 支持 Express、Koa、Webpack
4. 支持模板继承与子模板
5. 浏览器版本仅 6KB 大小

## 模板

art-template 同时支持两种模板语法。标准语法可以让模板更容易读写；原始语法具有强大的逻辑处理能力。

**标准语法**

```js
{{if user}}
  <h2>{{user.name}}</h2>
{{/if}}
```

**原始语法**

```js
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

原始语法兼容 [EJS](http://ejs.co/)、[Underscore](http://underscorejs.org/#template)、[LoDash](https://lodash.com/docs/#template) 模板。

## 渲染模板

```js
var template = require('art-template');
var html = template(__dirname + '/tpl-user.art', {
    user: {
        name: 'aui'
    }
});
```

## 核心方法

```js
// 基于模板名渲染模板
template(filename, data);

// 将模板源代码编译成函数
template.compile(source, options);

// 将模板源代码编译成函数并立刻执行
template.render(source, data, options);
```

## node使用

`tpl.html`

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>{{ title }}</title>
</head>
<body>
  <p>大家好，我叫：{{ name }}</p>
  <p>我今年 {{ age }} 岁了</p>
  <h1>我来自 {{ province }}</h1>
  <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
  <script>
    var foo = '{{ title }}'
  </script>
</body>
</html>

```

```js

// 安装：
//    npm install art-template
//    该命令在哪执行就会把包下载到哪里。默认会下载到 node_modules 目录中
//    node_modules 不要改，也不支持改。

// 在 Node 中使用 art-template 模板引擎
// 模板引起最早就是诞生于服务器领域，后来才发展到了前端。
// 
// 1. 安装 npm install art-template
// 2. 在需要使用的文件模块中加载 art-template
//    只需要使用 require 方法加载就可以了：require('art-template')
//    参数中的 art-template 就是你下载的包的名字
//    也就是说你 isntall 的名字是什么，则你 require 中的就是什么
// 3. 查文档，使用模板引擎的 API


var template = require('art-template')
var fs = require('fs')

// 这里不是浏览器
// template('script 标签 id', {对象})

// var tplStr = `
// <!DOCTYPE html>
// <html lang="en">
// <head>
//   <meta charset="UTF-8">
//   <title>Document</title>
// </head>
// <body>
//   <p>大家好，我叫：{{ name }}</p>
//   <p>我今年 {{ age }} 岁了</p>
//   <h1>我来自 {{ province }}</h1>
//   <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
// </body>
// </html>
// `

//注意这里的引用文件路径!!!依据自己实际存放/新建的位置
fs.readFile('./tpl.html', function (err, data) {
  if (err) {
    return console.log('读取文件失败了')
  }
  // 默认读取到的 data 是二进制数据
  // 而模板引擎的 render 方法需要接收的是字符串
  // 所以我们在这里需要把 data 二进制数据转为 字符串 才可以给模板引擎使用
  var ret = template.render(data.toString(), {
    name: 'zhao',
    age: 18,
    province: '湖北',
    hobbies: [
      '写代码',
      '看书',
      '打游戏'
    ],
    title: '个人信息'
  })

  console.log(ret)
})

```

## 浏览器使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>06-在浏览器中使用art-template</title>
</head>
<body>
  <!-- 
    注意：在浏览器中需要引用 lib/template-web.js 文件

    强调：模板引擎不关心你的字符串内容，只关心自己能认识的模板标记语法，例如 {{}}
    {{}} 语法被称之为 mustache 语法，八字胡啊。
   -->
     <!-- 注意这里的引用路径 -->
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script type="text/template" id="tpl">
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Document</title>
    </head>
    <body>
      <p>大家好，我叫：{{ name }}</p>
      <p>我今年 {{ age }} 岁了</p>
      <h1>我来自 {{ province }}</h1>
      <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
    </body>
    </html>
  </script>
  <script>
    var ret = template('tpl', {
      name: 'zy',
      age: 18,
      province: '北京市',
      hobbies: [
        '写代码',
        '唱歌',
        '打游戏'
      ]
    })

    console.log(ret)
  </script>
</body>
</html>

```

