---
title: Node-studyDay3
date: 2020-12-20 18:30:34
tags: Node
categories: Node
description: Node中的js-核心模块
---

Node为JavaScript:提供了很多服务器级别的API，这些API绝大多数都被包装到了一个具名的核心模块中了。例如文件操作的`|fs]`核心模块，http服务构建的`[http]`模块，`path `路径操作模块、`os`操作系统信息模块…

```javascript
// 用来获取机器信息的
var os = require('os')

// 用来操作路径的
var path = require('path')

// 获取当前机器的 CPU 信息
console.log(os.cpus())

// memory 内存
console.log(os.totalmem())

// 获取一个路径中的扩展名部分
// extname extension name
console.log(path.extname('c:/users/hello.txt'));

```

#### 简单的模块🌰

 require是一个方法,它的作用就是用来加载模块的

**在Node中，模块有三种:**

一、原生模块
Node.js自带的模块，属于Node.js本身的一些方法属性。

例如:

```markdown
1、http模块
2、fs模块
磁盘操作，文件操作
3、url模块
处理 url型的字符串
```

二、第三方模块
俗称包，依赖等，例如npm商店中下载的jQuery，用于方便我们操作的nodemon等脚本都属于第三方模块。

三、自定义模块
此类模块是自己命名定义的，尽量不与原生模块的命名发生冲突，自定义模块大多用于完成自己的项目需求。

<font style="color:deepskyblue;font-size:18px;">相对路径必须加`./`</font>

举个🌰

```nginx
  └── simpleModule
		├── a.js
		├── b.js
```

`a.js`

```js
console.log("a is start !");
require('./b');//这里文件后缀名.js可以省略
console.log("a is end");
```

`b.js`

```js
console.log("b被加载执行啦~");
```

![module_node7172e8380e3b8915.png](https://cdn.longdoer.com/2020/12/20/module_node7172e8380e3b8915.png)

在 Node 中，没有全局作用域，只有模块作用域

👆代码示例目录结构如上

`a.js`

```js
//      外部访问不到内部
//      内部也访问不到外部
//      默认都是封闭的
//    既然是模块作用域，那如何让模块与模块之间进行通信
//    有时候，我们加载文件模块的目的不是为了简简单单的执行里面的代码，更重要是为了使用里面的某个成员
var foo = 'aaa'

console.log("a is start !");

require('./b');

console.log("a is end");

console.log('foo 的值是：', foo)
```

`b.js`

```js
console.log('b start')

var foo = 'bbb'

console.log('b end')
```

![module_node1488e9fb0dbe50c86.png](https://cdn.longdoer.com/2020/12/20/module_node1488e9fb0dbe50c86.png)

🐛如果我们尝试定义一个函数,然后在另一个模块调用了呢?

`a.js`

```js
var foo = 'aaa'

console.log("a is start !");

function add(a,b){
    return a + b;
}

require('./b');

console.log("a is end");

console.log('foo 的值是：', foo)
```

`b.js`

```js
console.log('b start')

var foo = 'bbb'

console.log(add(1,2));

console.log('b end')
```

输出结果:

```js
console.log(add(1,2));
        ^
ReferenceError: add is not defined//add函数未定义
```

很显然,函数也行不通emmm



#### 简单的模块加载与导出

require 方法有两个作用：

  \1. 加载文件模块并执行里面的代码

  \2. 拿到被加载文件模块导出的接口对象

目录:

```nginx
  └── requireAndImport
		├── a.js
		├── b.js
```

`a.js`

```js
require('./b');
console.log(foo);
```

`b.js`

```
var foo = 'zy';
```

输出:

```js
console.log(foo);
            ^
ReferenceError: foo is not defined
```

  在每个文件模块中都提供了一个对象：`exports`

`  exports `默认是一个空对象

 你要做的就是把所有需要被外部访问的成员挂载到这个` exports` 对象中

代码例:[ 目录结构如上 ]

`a.js`

```js
var value = require('./b');

console.log(value);//{ foo: 'hello' }
console.log(value.foo);//hello
```

`b.js`

```js
var foo = 'zy';

exports.foo = 'hello';
```

输出:

```js
{ foo: 'hello' }
hello
```

**函数导出简单例子:**

`a.js`

```js
var value = require('./b');

console.log(value.add(520,99));
```

`b.js`

```js
var foo = 'zy';

exports.add = function (x, y) {
    return x + y;
}
```

输出:

```js
619
```

**变量导出简单例子:**

`a.js`

```js
var value = require('./b');

console.log(value.age);
```

`b.js`

```js
var age = 18;
exports.age = age;//如果没有这句导出,a.js的输出就是undefined
//原因嘛,前面你看了,你懂得
```

输出:

```js
18
```

