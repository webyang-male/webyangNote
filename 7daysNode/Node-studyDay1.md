---
title: Node studyDay1
date: 2020-12-19 14:46:56
tags: Node
categories: Node
description: Node学习之路就此起航!
---

在 Node中，采用 EcmaScript进行编码 ——  没有 BOM、DOM,和浏览器中的`Javascript`不一样

```js
console.log(window);
console.log(document);
```

<!-- more -->  

node 命令编译结果:

```javascript
console.log(window);
            ^

ReferenceError: window is not defined
    at Object.<anonymous> (E:\tzktWebProject\kj\node\node1.js:1:13)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
```

当然会报错啦!

<font style="color:deepskyblue;">浏览器中的 JavaScript是没有文件操作的能力的但是 Node中的JavaScript具有文件操作的能力</font>

- fs是`file-system`的简写,就是文件系统的意思
- 在Node中如果想要进行文件操作，就必须引入fs 这个核心模块
- 在fs这个核心模块中，就提供了所有的文件操作相关的AP工   例如:` fs.readFile`就是用来读取文件的

#### 读文件

代码🌰:

```nginx
//1.使用require方法加载fs核心模块
var fs = require('fs');

//2.读取文件
//第一个参数是---需要读取的文件路径
//第二个参数是---一个回调函数
/*
回调函数接收2个参数:
error
    如果读取失败，error就是错误对象
    如果读取成功，error就是null

data
    如果读取失败，error就是错误对象
    如果读取成功，data就是读取到的数据

    成功
        data 数据
        error null
    失败
        data null
        error 错误对象

*/
fs.readFile('fs.txt', function (error, data) {
    //<Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 2e 6a 73 21 0d 0a 20 20 20 20 20 20 2d 2d 57 72 69 74 74 65 6e 20 62 79 20 7a 7a 79 2e>
    /*  
     这里打印出来的data结果不是乱码,是一个二进制数据 0 / 1 
     文件中存储的其实都是二进制数据 0 / 1
     这里为什么看到的不是0和1呢?原因是二进制转为16进制了.但是无论是2进制还是 16进制,人类都不认识
     所以我们可以通过 toString 方法把其转为我们能认识的字符
     */
    console.log(data);
    console.log(data.toString());
})
```

![nodefs4d94b8a69972a7c7.png](https://cdn.longdoer.com/2020/12/19/nodefs4d94b8a69972a7c7.png)

#### 写文件

(还是上面的目录结构哦亲)

````js
//1.使用require方法加载fs核心模块
var fs = require('fs');

//第一个参数:文件路径
//第二个参数:文件内容

/* 
成功:
    文件写入成功
    error是null
失败:
    文件写入失败
    error就是错误对象 
*/
fs.writeFile('fs.txt', '大家好鸭~我是Node.js!', function (error) {
    console.log("文件写入成功啦!");
})
````

#### 思考

假设我们尝试读取一个不存在的文件,会报错嘛?

```javascript
fs.readFile('fs1.txt', function (error, data) {
   //啥反应也没有,艹
})
```

我们打印一下:

```javascript
//1.使用require方法加载fs核心模块
var fs = require('fs');

//2.读取文件
//第一个参数是---需要读取的文件路径
//第二个参数是---一个回调函数
/*
回调函数接收2个参数:
error
    如果读取失败，error就是错误对象
    如果读取成功，error就是null

data
    如果读取失败，error就是错误对象
    如果读取成功，data就是读取到的数据

    成功
        data 数据 undefined--没有数据
        error null
    失败
        data null
        error 错误对象

*/
//fs1.txt这个文件是不存在的啦
fs.readFile('fs1.txt', function (error, data) {
    console.log(error);//error是一个对象
    console.log(data);//undefined
   
})
```

输出结果:

```javascript
[Error: ENOENT: no such file or directory, open 'E:\tzktWebProject\kj\node\fs1.txt'] {
  errno: -4058,
  code: 'ENOENT',
  syscall: 'open',
  path: 'E:\\tzktWebProject\\kj\\node\\fs1.txt'
}
undefined
```

所以这样的代码是不合理的,没有文件,你就要给我一个报错的响应啊!

读操作代码优化:

```nginx
//1.使用require方法加载fs核心模块
const { log } = require('console');
var fs = require('fs');

//2.读取文件
//第一个参数是---需要读取的文件路径
//第二个参数是---一个回调函数
/*
回调函数接收2个参数:
error
    如果读取失败，error就是错误对象
    如果读取成功，error就是null

data
    如果读取失败，error就是错误对象
    如果读取成功，data就是读取到的数据

    成功
        data 数据
        error null
    失败
        data null
        error 错误对象

*/
//在这里就可以通过判断error来确认是否有错误发生
fs.readFile('fs.txt', function (error, data) {
    if (error) {
        console.log("读取文件失败,可能是文件不存在了呢");
        return;
    } else {
        console.log(data.toString());
    }
})
```

写操作也是上述思路哦👆