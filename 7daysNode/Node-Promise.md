---
title: Node-Promise
date: 2020-12-26 09:14:59
tags: Promise
categories: Node
description: Node学习中涉及到的es6 Promise
---

#### 回调地狱

**目录**

```markdown
├── data
	├── a.txt
	├── b.txt
	├── c.txt
├── callback_hell.js
```

`callback_hell.js`

```js
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function (err, data) {
  if (err) {
    // return console.log('读取失败')
    // 抛出异常
    //    1. 阻止程序的执行
    //    2. 把错误消息打印到控制台
    throw err
  }
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      // return console.log('读取失败')
      // 抛出异常
      //    1. 阻止程序的执行
      //    2. 把错误消息打印到控制台
      throw err
    }
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
      if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
      }
      console.log(data)
    })
  })
})

```

⚠️这里的异步代码输出的结果不一定保证按顺序执行,即使文件(内容)大小一致.

​	理论文件内容越多,执行时间越长,输出越靠后(这个也不一定)

------

为了保证代码结果按顺序输出,代码进行嵌套

这时就形成了`回调地狱`:

```js
var fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', function (err, data) {
  if (err) {
    // return console.log('读取失败')
    // 抛出异常
    //    1. 阻止程序的执行
    //    2. 把错误消息打印到控制台
    throw err
  }
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      // return console.log('读取失败')
      // 抛出异常
      //    1. 阻止程序的执行
      //    2. 把错误消息打印到控制台
      throw err
    }
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
      if (err) {
        // return console.log('读取失败')
        // 抛出异常
        //    1. 阻止程序的执行
        //    2. 把错误消息打印到控制台
        throw err
      }
      console.log(data)
    })
  })
})

```

![](https://ftp.bmp.ovh/imgs/2020/12/cbb80eb5ca3ab209.png)

#### Promise

为了解决以上编码方式带来的问题（回调地狱嵌套），所以在EcmaScript 6中新增了一个 API —– Promise 
	   Promise的英文就是承诺、保证的意思（l promise you)

一个 Promise 必然处于以下几种状态之一：

>  待定（pending）: 初始状态，既没有被兑现，也没有被拒绝。
> 		已兑现（fulfilled）: 意味着操作成功完成。
> 		已拒绝（rejected）: 意味着操作失败。

```js
  1). Promise构造函数: Promise (excutor) {}  
  2). excutor函数: 同步执行  (resolve, reject) => {}  
  3). resolve函数: 内部定义成功时我们调用的函数 value => {}  
  4). reject函数: 内部定义失败时我们调用的函数 reason => {}  
  5). Promise.prototype.then方法: (onResolved, onRejected) => {}  
  6). onResolved函数: 成功的回调函数  (value) => {}  
  7). onRejected函数: 失败的回调函数 (reason) => {}  
  8). Promise.prototype.catch方法: (onRejected) => {}  
  9). Promise.resolve方法: (value) => {}  
  10). Promise.reject方法: (reason) => {}  
  11). Promise.all方法: (promises) => {}  

```

##### `promiseApi.js`

```js
//Promise是一个构造函数

var fs = require('fs')

console.log(1);

//创建Promise容器
//Promise容器一旦创建,就开始执行里面的代码
new Promise(function () {
    console.log(2);
    fs.readFile('./data/a.txt', 'utf8', function (err, data) {
        if (err) {
            //失败了,承诺容器中的任务失败了
            console.log(err);
        } else {
            console.log(3);
            //承诺容器中的任务成功了
            console.log(data);
        }
    })
})
console.log(4);



/* 
输出结果:
1
2
4
3
hello aaa */
```

一个简单Promise实例

```js
var fs = require('fs')

var p1 = new Promise(function (resolve, reject) {
    fs.readFile('./data/a.txt', 'utf8', function (err, data) {
        if (err) {
            reject(err)
        } else {
            //   resolve(data)
            resolve(123)
        }
    })
})
p1
    .then(function (data) {
        //p1就是那个承诺
        //当p1成功然后(then)做指定的操作
        //then 方法接收的 function 就是容器中的 resolve函数
        console.log(data);//输出123

    })

```

##### Promise基本语法

```js
var fs = require("fs");

var p1 = new Promise(function (resolve, reject) {
  fs.readFile("./data/a.txt", "utf8", function (err, data) {//异步任务
    if (err) {
      //失败调用
      reject(err);
    } else {
    //成功调用
      resolve(data);
    }
  });
});
p1.then(
  function (data) {
    //p1就是那个new出来的Promise实例
    //当p1成功然后(then)做指定的操作
    //then 方法接收的 function 就是容器中的 resolve函数
    console.log(data);
  },
  function (err) {//这里接收的 function 就是容器中的 reject函数
    console.log("文件读取失败", err);
  }
);

```

此时来解决前面的回调地狱嵌套

```js
var fs = require('fs')

var p1 = new Promise(function (resolve, reject) {
  fs.readFile('./data/a.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

var p2 = new Promise(function (resolve, reject) {
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

var p3 = new Promise(function (resolve, reject) {
  fs.readFile('./data/c.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

p1
  .then(function (data) {
    console.log(data)
    // 当 p1 读取成功的时候
    // 当前函数中 return 的结果就可以在后面的 then 中 function 接收到
    // 当你 return 123 后面就接收到 123
    //      return 'hello' 后面就接收到 'hello'
    //      没有 return 后面收到的就是 undefined
    // 上面那些 return 的数据没什么卵用
    // 真正有用的是：我们可以 return 一个 Promise 对象
    // 当 return 一个 Promise 对象的时候，后续的 then 中的 方法的第一个参数会作为 p2 的 resolve
    // 
    return p2
  }, function (err) {
    console.log('读取文件失败了', err)
  })
  .then(function (data) {
    console.log(data)
    return p3
  })
  .then(function (data) {
    console.log(data)
    console.log('end')
  })

```

##### 简易封装Promise版本的readFile

```js
var fs = require('fs')

function pReadFile(filePath) {
  return new Promise(function (resolve, reject) {
    fs.readFile(filePath, 'utf8', function (err, data) {
      if (err) {
        reject(err)
      } else {
        resolve(data)
      }
    })
  })
}

pReadFile('./data/a.txt')
  .then(function (data) {
    console.log(data)
    return pReadFile('./data/b.txt')
  })
  .then(function (data) {
    console.log(data)
    return pReadFile('./data/c.txt')
  })
  .then(function (data) {
    console.log(data)
  })

```

##### 简易封装Promise版本的ajax方法

```js
    //封装后的callback写法
		// pGet('http://127.0.0.1:3000/users/4', function (data) {
    //   console.log(data)
    // })
		
		//封装后的Promise写法
    pGet('http://127.0.0.1:3000/users/4')
      .then(function (data) {
        console.log(data)
      })

	function pGet(url, callback) {
      return new Promise(function (resolve, reject) {
        var oReq = new XMLHttpRequest()
        // 当请求加载成功之后要调用指定的函数
        oReq.onload = function () {
          // 我现在需要得到这里的 oReq.responseText
          callback && callback(JSON.parse(oReq.responseText))
          resolve(JSON.parse(oReq.responseText))
        }
        oReq.onerror = function (err) {
          reject(err)
        }
        oReq.open("get", url, true)
        oReq.send()
      })
    }


		// 这个 get 是 callback 方式的 API
    // 可以使用 Promise 来解决这个问题
    function get(url, callback) {
      var oReq = new XMLHttpRequest()
      // 当请求加载成功之后要调用指定的函数
      oReq.onload = function () {
        // 我现在需要得到这里的 oReq.responseText
        callback(oReq.responseText)
      }
      oReq.open("get", url, true)
      oReq.send()
    }



```





#### 推荐文章:

[ES6 Promise 解析及详解三个状态]: https://ainyi.com/16


阮一峰ES6