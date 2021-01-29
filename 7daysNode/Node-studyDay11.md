---
title: Node-studyDay11
date: 2020-12-24 21:57:01
tags: Node
categories: Node
description: express中的static-server静态资源服务和callback初窥
---

#### 基本路由

##### 路由器

- 请求方法
- 请求路径
- 请求处理函数

##### get:

```js
//当你以GET方法请求/的时候，执行对应的处理函数
app.get('/', function (req, res) {
    res.send('hello express!');//express推荐写法res.send()
})
```

##### post:

```js
//当你以POST方法请求/的时候，执行对应的处理函数
app.post('/', function (req, res) {
    res.send('hello express post!');//express推荐写法res.send()
})
```

#### 静态服务

目录

[![express_node9075d568306f6988.png](https://b2.kuibu.net/file/imgdisk/2020/12/24/express_node9075d568306f6988.png)](https://img.kuibu.net/image/jUvM5)

```javascript
var express = require('express')
//1.创建app
var app = express()
第一种
//当以/public/开头的时候，去./poblic/目录查找对应的资源
app.use('/public/',express.static('./public/'))
//可以直接访问127.0.01：3000/public/index.html
第二种
//当省略第一个参数时，则可以通过省略/public的方式来访问
app.use(express.static('./public/'))
//可以直接访问127.0.01：3000/index.html
//127.0.01：3000/public/index.html这个url则会报错
第三种
//必须是/a/public/中的资源具体路径
app.use('/abc/',express.static('./public/'))
//127.0.01：3000/abc/index.html才能访问public的东西，可以理解为abc是public的别名
app.get('/',function(req,res){
	res.send('helloworld')
})

app.listien(300,function(){
    console.log('running...')
})
```

#### 回调函数callback

```js
// 函数也是一种数据类型
// 参数
// 返回值
// 函数太灵活了，无所不能
// 一般情况下，把函数作为参数的目的就是为了获取函数内部的异步操作结果
function add(x, y) {
  return x + y
}
add(10, 20)
```

```js
// JavaScript 单线程、事件循环
//例1
console.log(1)

// 不会等待
setTimeout(function () {
  console.log(2)
  console.log('hello')
}, 0)

console.log(3)
//1 3 2 hello


//不成立的情况
//例2
function add(x, y) {
  console.log(1)
  setTimeout(function () {
    console.log(2)
    var ret = x + y
    return ret
  }, 1000)
  console.log(3);
  //到这里执行就结束了，不会等到前面的定时器，所以直接就返回了默认值 undefined
}

console.log(add(10,20));
//1 3 undefined 2


//例3
function add(x, y) {
  var ret;
  console.log(1);
  setTimeout(function () {
    console.log(2);
    var ret = x + y;
    return ret;
  }, 1000)
  console.log(3);
  return ret;
}

console.log(add(10,20));
//1 3 undefined 2


//成立情况
var ret;
function add(x, y) {
  console.log(1);
  setTimeout(function () {
    console.log(2);
    ret = x + y;
  }, 1000)
}
add(10,20);
//这里不一定要2000ms,只要保证在add()调用之后执行就可
setTimeout(function(){
  console.log(ret);
},2000);
```

```javascript
// 注意：凡是需要得到一个函数内部异步操作的结果
   setTimeout
   readFile
   writeFile
   ajax
// 这种情况必须通过：回调函数
```

**回调函数**🌰

```js
function add(x, y, callback) {
  //callback就是回调函数
  //var x = 10;
  //var y = 20;
  //var callback = function(ret){console.log(ret)}
  
  console.log(1)
  setTimeout(function () {
    var ret = x + y
    callback(ret)
  }, 1000)
}

add(10, 20, function (ret) {
  console.log(ret)
})
//1,30
```

#### package-lock.json

npm5以前，没有package-lock.json这个文件
	   npm5以后才加入这个文件的

当安装包的时候，npm都会生成或者更新package-lock.json这个文件
•	npm5以后的版本安装包不需要加–save参数，它会自动保存依赖信息
•	当安装包的时候，会自动创建或者更新package-lock.json这个文件
•	package-lock.json这个文件会自动保存node_modules中所包含的信息(版本，下载地址)
•	这样的话重新npm install的时候速度就可以提升


•	从文件来看，有一个lock称之为锁
•	这个lock是用来锁定版本的


•	如果项目依赖了1.1.1版本
•	在没有package-lock.json文件的情况下，重新npm install其实会下载最新的版本，而不是 1.1.1
•	我们的目的是希望可以锁住1.1.1这个版本
•	所以这个package-lock.json文件的另一个作用是锁定版本，防止自动升级新版

