---
title: Node studyDay2
date: 2020-12-19 16:08:23
tags: Node
categories: Node
description: 简单的http服务
---

<font style="color:deepskyblue;">在Node中专门提供了一个核心模块: http</font>

<!-- more -->  

http 这个模块的职责就是帮你创建编写服务器的

```nginx
//1.加载http核心模块
var http = require('http');

// 2.使用http.createServer()方法创建一个web服务器
// 返回一个Server实例
var server = http.createServer();

//3.服务器--提供对数据的服务
// 发请求
// 接收请求
// 处理请求
// 给个反馈(发送响应)
// 注册request请求事件
// 当客户端请求过来，就会自动触发服务器的request请求事件，然后执行第二个参数:回调处理函数
server.on('request', function () {
    console.log('收到客户端的请求啦!');
})

//4.绑定端口号,启动服务器
server.listen(3000, function () {
    console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/来访问');
})
```

![http_node6b80bcc5239309fa.png](https://cdn.longdoer.com/2020/12/19/http_node6b80bcc5239309fa.png)

✏️这时候你通过这个链接打开网页,hi发现网页一直在转圈,是因为我们的服务器没有对浏览器(客户端)进行回复,所以浏览器一直在等待

tips:本地终端`ctr + c`快捷键可以终止服务

### 发送响应

```nginx
//1.加载http核心模块
var http = require('http');

// 2.使用http.createServer()方法创建一个web服务器
// 返回一个Server实例
var server = http.createServer();

//3.服务器--提供对数据的服务
/* request 请求事件处理函数，需要接收两个参数:
    Request请求对象
    请求对象可以用来获取客户端的一些请求信息，例如请求路径
    Response 响应对象
    响应对象可以用来给客户端发送响应消息 */
server.on('request', function (request, response) {
    //  收到客户端的请求啦!请求路径是:/
    console.log('收到客户端的请求啦!请求路径是:' + request.url);
})

//4.绑定端口号,启动服务器
server.listen(3000, function () {
    console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/来访问');
})
```

#### 响应回复

```nginx
//1.加载http核心模块
var http = require('http');

// 2.使用http.createServer()方法创建一个web服务器
// 返回一个Server实例
var server = http.createServer();

//3.服务器--提供对数据的服务
/* request 请求事件处理函数，需要接收两个参数:
    Request请求对象
    请求对象可以用来获取客户端的一些请求信息，例如请求路径
    Response 响应对象
    响应对象可以用来给客户端发送响应消息 */
server.on('request', function (request, response) {
    //收到客户端的请求啦!请求路径是:/
    console.log('收到客户端的请求啦!请求路径是:' + request.url);

    //response对象有一个方法: write可以用来给客户端发送响应数据
    //write可以使用多次，但是最后一定要使用end 来结束响应，否则客户端会一直等待
    response.write('zy!');
    response.write('node');

    //end告诉客户端，我的话说完了，你可以呈递给用户了
    response.end();
})

//4.绑定端口号,启动服务器
server.listen(3000, function () {
    console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/来访问');
})
```

这里无论页面路径是什么,页面返回的信息都是`zy!node`

举个🌰

```nginx
url																									response
http://127.0.0.1:3000/																zy!node
http://127.0.0.1:3000/1																zy!node
http://127.0.0.1:3000/xyz															zy!node
http://127.0.0.1:3000/xyz/12%$										     zy!node
```

### 🤔思考                

#### 根据不同请求路径返回不同结果

```nginx
//1.加载http核心模块
//1.创建 Server
var http = require('http');
const server = http.createServer();


//2.监听 request请求事件,设置请求处理函数
server.on('request', function (req, res) {
    console.log('收到客户端的请求啦!请求路径是:' + req.url);

    /* 
    //这里的方式比较麻烦,推荐更简单的方法,直接end的同时发送响应数据
    res.write('hello');
    res.write(' node');
    res.end(); 
    */
    //简单方法
    //res.end('hello Node'); 

    //根据不同的请求路径发送不同的响应结果
    //1．获取请求路径
    //req.url获取到的是端口号之后的那一部分路径
    //也就是说所有的url都是以/开头的
    //2．判断路径处理响应
    var url = req.url;
    if (url === '/') {
        res.end("index page");
    } else if (url === '/login') {
        res.end("login page");
    } else {
        res.end("404 Not Found");
    }

});

//3.绑定端口号,启动服务器
server.listen(3000, function () {
    console.log('服务器启动成功了，可以访问 http://127.0.0.1:3000/');
})

```

##### 返回一个数组数据

```nginx
var http = require('http')

// 1. 创建 Server
var server = http.createServer()

// 2. 监听 request 请求事件，设置请求处理函数
server.on('request', function (req, res) {
  console.log('收到请求了，请求路径是：' + req.url)
  console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)

  // res.write('hello')
  // res.write(' world')
  // res.end()

  // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
  // res.end('hello nodejs')

  // 根据不同的请求路径发送不同的响应结果
  // 1. 获取请求路径
  //    req.url 获取到的是端口号之后的那一部分路径
  //    也就是说所有的 url 都是以 / 开头的
  // 2. 判断路径处理响应

  var url = req.url

  if (url === '/') {
    res.end('index page')
  } else if (url === '/login') {
    res.end('login page')
  } else if (url === '/products') {
    var products = [{
        name: '苹果 X',
        price: 8888
      },
      {
        name: '小米 Alpha',
        price: 18000
      },
      {
        name: 'oppo R7',
        price: 1999
      }
    ]

    // 响应内容只能是二进制数据或者字符串
    //  数字
    //  对象
    //  数组
    //  布尔值
    res.end(JSON.stringify(products));//试试看parse
  } else {
    res.end('404 Not Found.')
  }
})

// 3. 绑定端口号，启动服务
server.listen(3000, function () {
  console.log('服务器启动成功，可以访问了。。。')
})

```

