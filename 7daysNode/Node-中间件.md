---
title: Node-中间件
date: 2020-12-26 16:09:57
tags: Node
categories: Node
description: Express中间件初窥
---

**中间件概念**

`express-middleware.js`

```nginx
var http = require('http')
var url = require('url')

var cookie = require('./middlewares/cookie')
var postBody = require('./middlewares/post-body')
var query = require('./middlewares/query')
var session = require('./middlewares/session')

var server = http.createServer(function (req, res) {
  // 解析表单 get 请求体
  // 解析表单 post 请求体
  // 解析 Cookie
  // 处理 Session
  // 使用模板引擎
  // console.log(req.query)
  // console.log(req.body)
  // console.log(req.cookies)
  // console.log(req.session)

  // 解析请求地址中的 get 参数
  // var urlObj = url.parse(req.url, true)
  // req.query = urlObj.query
  query(req, res)

  // 解析请求地址中的 post 参数
  // req.body = {
  //   foo: 'bar'
  // }
  postBody(req, res)

  // 解析 Cookie
  // req.cookies = {
  //   isLogin: true
  // }
  cookie(req, res)

  // 配置 Session
  // req.session = {}
  session(req, res)

  // 配置模板引擎
  res.render = function () {
    
  }

  if (req.url === 'xxx') {
    // 处理
    // query、body、cookies、session、render API 成员
  } else if (url === 'xx') {
    // 处理
  }


  // 上面的过程都是了为了在后面做具体业务操作处理的时候更方便
})

server.listen(3000, function () {
  console.log('3000. running...')
})

```

**Express中的中间件**

`express-middleware1.js`

```js
var express = require('express')

var app = express()

// 中间件：处理请求的，本质就是个函数

// 在 Express 中，对中间件有几种分类

// 当请求进来，会从第一个中间件开始进行匹配
//    如果匹配，则进来
//       如果请求进入中间件之后，没有调用 next 则代码会停在当前中间件
//       如果调用了 next 则继续向后找到第一个匹配的中间件
//    如果不匹配，则继续判断匹配下一个中间件
//    
// 不关心请求路径和请求方法的中间件
// 也就是说任何请求都会进入这个中间件
// 中间件本身是一个方法，该方法接收三个参数：
//    Request 请求对象
//    Response 响应对象
//    next     下一个中间件
// 当一个请求进入一个中间件之后，如果不调用 next 则会停留在当前中间件
// 所以 next 是一个方法，用来调用下一个中间件的
// 调用 next 方法也是要匹配的（不是调用紧挨着的那个）

// app.use(function (req, res, next) {
//   console.log('1')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('2')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('3')
//   res.send('333 end.')
// })

// app.use(function (req, res, next) {
//   console.log(1)
//   next()
// })


// app.use('/b', function (req, res, next) {
//   console.log('b')
// })

// 以 /xxx 开头的路径中间件
// app.use('/a', function (req, res, next) {
//   console.log('a')
//   next()
// })

// app.use(function (req, res, next) {
//   console.log('2')
//   next()
// })

// app.use('/a', function (req, res, next) {
//   console.log('a 2')
// })

// 除了以上中间件之外，还有一种最常用的
// 严格匹配请求方法和请求路径的中间件
// app.get
// app.post

app.use(function (req, res, next) {
  console.log(1)
  next()
})

app.get('/abc', function (req, res, next) {
  console.log('abc')
  next()
})

app.get('/', function (req, res, next) {
  console.log('/')
  next()
})

app.use(function (req, res, next) {
  console.log('haha')
  next()
})

app.get('/abc', function (req, res, next) {
  console.log('abc 2')
})

app.use(function (req, res, next) {
  console.log(2)
  next()
})

app.get('/a', function (req, res, next) {
  console.log('/a')
})

app.get('/', function (req, res, next) {
  console.log('/ 2')
})

// 如果没有能匹配的中间件，则 Express 会默认输出：Cannot GET 路径

app.listen(3000, function () {
  console.log('app is running at port 3000.')
})
```

**配置中间件**

```js
var express = require('express')
var fs = require('fs')

var app = express()

// app.get('/abc', function (req, res, next) {
//   console.log('abc')
//   // req.foo = 'bar'
//   req.body = {}
//   next()
// })

// app.get('/abc', function (req, res, next) {
//   console.log(req.body)
//   console.log('abc 2')
// })

app.get('/', function (req, res, next) {
  fs.readFile('.d/sa./d.sa/.dsa', function (err, data) {
    if (err) {
      // return res.status(500).send('Server Error')
      // 当调用 next 的时候，如果传递了参数，则直接往后找到带有 四个参数的应用程序级别中间件
      // 当发生错误的时候，我们可以调用 next 传递错误对象
      // 然后就会被全局错误处理中间件匹配到并处理之
      next(err)
    }
  })
})

app.get('/', function (req, res, next) {
  console.log('/ 2')
})



app.get('/a', function (req, res, next) {
  fs.readFile('./abc', function (err, data) {
    if (err) {
      // return res.status(500).send('Server Error') 
      next(err)
    }
  })
})

app.use(function (req, res, next) {
  res.send('404')
})

// 配置错误处理中间件
app.use(function (err, req, res, next) {
  res.status(500).send(err.message)
})

app.listen(3000, function () {
  console.log('app is running at port 3000.')
})

```

