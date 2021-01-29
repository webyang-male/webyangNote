---
title: Node-studyDay6
date: 2020-12-23 11:50:15
tags: Node
categories: Node
description: 客户端渲染和服务端渲染/模块系统
---

#### 客户端渲染流程简图

![ea0e1906df9035d409ec6d1d8ec642a11b207a334f87a2f5.png](https://cdn.longdoer.com/2020/12/23/ea0e1906df9035d409ec6d1d8ec642a11b207a334f87a2f5.png)

#### 服务端渲染流程简图

![eb59338b6e307e4d557c15b87248ac01f8cd08c4426dcb53.png](https://cdn.longdoer.com/2020/12/23/eb59338b6e307e4d557c15b87248ac01f8cd08c4426dcb53.png)

### 模块系统

#### Node.js 的模块

JavaScript 做为一门为网页添加交互功能的简单脚本语言问世，在诞生时并不包含模块系统，随着 JavaScript 解决问题越来越复杂，把所有代码写在一个文件内，用 function 区分功能单元已经不能支撑复杂应用开发了，ES6 带来了大部分高级语言都有的 class 和 module，方便开发者组织代码

```js
import _ from 'lodash';

class Fun {}

export default Fun;
```

上面三行代码展示了一个模块系统最重要的两个要素 import 和 export

1. `export`用于规定模块的对外接口
2. `import`用于输入其他模块提供的功能



而在 ES6 之前，社区出现了很多模块加载方案，最主要的有 CommonJS 和 AMD 两种，Node.js 诞生早于 ES6，模块系统使用的是类似 CommonJS 的实现，遵从几个原则

1. 一个文件是一个模块，文件内的变量作用域都在模块内
2. 使用 `module.exports` 对象导出模块对外接口
3. 使用 `require` 引入其它模块



```js
circle.js
const { PI } = Math;

module.exports = function area(r) {
  return PI * r ** 2;
};
```

上面代码就实现了 Node.js 的一个模块，模块没有依赖其它模块，导出了方法 `area` 计算圆的面积



```js
test.js
const area = require('./circle.js');
console.log(`半径为 4 的圆的面积是 ${area(4)}`);
```

模块依赖了 circle.js，使用其对外暴露的 area 方法，计算圆的面积

#### module.exports

模块对外暴露接口使用 module.exports，常见的有两种用法：为其添加属性或赋值到新对象

```js
test.js
// 添加属性
module.exports.prop1 = xxx;
module.exports.funA = xxx;
module.exports.funB = xxx;

// 赋值到全新对象
module.exports = {
  prop1,
    funA,
  funB,
};
```

两种写法是等价的，使用时候没区别

```js
const mod = require('./test.js');

console.log(mod.prop1);
console.log(mod.funA());
```

还有另外一种直接使用 `exports` 对象的方法，但是只能对其添加属性，不能赋值到新对象，后面会介绍原因

```js
// 正确的写法：添加属性
exports.prop1 = xxx;
exports.funA = xxx;
exports.funB = xxx;

// 赋值到全新对象
module.exports = {
  prop1,
    funA,
  funB,
};
```

#### require('id')

##### 模块类型

require 用法比较简单，id 支持模块名和文件路径两种类型

##### 模块名

```js
const fs = require('fs');
const _ = require('lodash');
```

示例中的 fs、lodash 都是模块名，fs 是 Node.js 内置的核心模块，lodash 是通过 npm 安装到 `node_modules` 下的第三方模块，如果出现重名，优先使用系统内置模块



因为一个项目内可能会包含多个 node_modules 文件夹（Node.js 比较失败的设计），第三方模块查找过程会遵循就近原则逐层上溯（可以在程序中打印 `module.paths` 查看具体查找路径），直到根据 `NODE_PATH` 环境变量查找到文件系统根目录，具体过程可以参考[官方文档](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_loading_from_node_modules_folders)



此外，Node.js 还会搜索以下的全局目录列表：

- $HOME/.node_modules
-  $HOME/.node_libraries
-  $PREFIX/lib/node

其中 `$HOME` 是用户的主目录， `$PREFIX` 是 Node.js 里配置的 `node_prefix`。强烈建议将所有的依赖放在本地的 node_modules 目录，这样将会更快地加载，且更可靠

##### 文件路径

模块还可以可以使用文件路径加载，这是项目内自定义模块的通用加载方式，路径可以省略拓展名，会按照 .js、.json、.node 顺序尝试

- 以 `'/'` 为前缀的模块是文件的绝对路径，按照系统路径查找模块
- 以 `'./'` 为前缀的模块是相对于当前调用 require 方法的文件，不受后续模块在哪里被使用到影响

#### 单次加载 & 循环依赖

模块在第一次加载后会被缓存到 `Module._cache` ，如果每次调用 `require('foo')` 都解析到同一文件，则返回相同的对象，同时多次调用 `require(foo)` 不会导致模块的代码被执行多次。 Node.js 根据实际的文件名缓存模块，因此从不同层级目录引用相同模块不会重复加载。



理解的模块单次加载机制方便我们理解模块循环依赖后的现象

```js
a.js
console.log('a 开始');
exports.done = false;
const b = require('./b.js');
console.log('在 a 中，b.done = %j', b.done);
exports.done = true;
console.log('a 结束');
b.js
console.log('b 开始');
exports.done = false;
const a = require('./a.js');
console.log('在 b 中，a.done = %j', a.done);
exports.done = true;
console.log('b 结束');
```

`main.js`:

```js
console.log('main 开始');
const a = require('./a.js');
const b = require('./b.js');
console.log('在 main 中，a.done=%j，b.done=%j', a.done, b.done);
```

当 main.js 加载 a.js 时，a.js 又加载 b.js,此时，b.js 会尝试去加载 a.js



为了防止无限的循环会返回一个 a.js 的 exports 对象的 **未完成的副本** 给 b.js 模块，然后 b.js 完成加载，并将 exports 对象提供给 a.js 模块



因此示例的输出是

```markdown
main 开始
a 开始
b 开始
在 b 中，a.done = false
b 结束
在 a 中，b.done = true
a 结束
在 main 中，a.done=true，b.done=true
```

看不懂上面的过程也没关系，日常工作根本用不到，即使看懂了也不要在项目中使用循环依赖！

### 工作原理

Node.js 每个文件都是一个模块，模块内的变量都是局部变量，不会污染全局变量，在执行模块代码之前，Node.js 会使用一个如下的函数封装器将模块封装

```js
(function(exports, require, module, __filename, __dirname) {
    // 模块的代码实际上在这里
});
```

- __filename：当前模块文件的绝对路径
-  __dirname：当前模块文件据所在目录的绝对路径
- module：当前的模块实例
- require：加载其它模块的方法，module.require 的快捷方式
- exports：导出模块接口的对象，module.exports 的快捷方式



回头看看最开始的问题，为什么 exports 对象不支持赋值为其它对象？把上面函数添加一句 exports 对象来源就很简单了

```js
const exports = module.exports;
(function(exports, require, module, __filename, __dirname) {
    // 模块的代码实际上在这里
});
```

其它模块 require 到的肯定是模块的 module.exports 对象，如果把 exports 对象赋值给其它对象，就和 module.exports 对象断开了连接，自然就没用了

### 在 Node.js 中使用 ES Module

随着 ES6 使用越来越广泛，Node.js 也支持了 ES6 Module，有几种方法

#### babel 构建

使用 babel 构建是在 v12 之前版本最简单、通用的方式，具体配置参考 [@babel/preset-env](https://babeljs.io/docs/en/babel-preset-env)



```jsx
.babelrc
{
  "presets": [
    ["@babel/preset-env", {
      "targets": {
        "node": "8.9.0",
        "esmodules": true
      }      
    }]
  ]
}
```

#### 原生支持

在 v12 后可以使用原生方式支持 ES Module

1. 开启 `--experimental-modules` 
2. 模块名修改为 `.mjs` （强烈不推荐使用）或者 package.json 中设置 `"type": module` 

这样 Node.js 会把 js 文件都当做 ES Module 来处理，更多详情参考[官方文档](https://nodejs.org/dist/latest-v13.x/docs/api/esm.html)

### 原文链接

https://www.yuque.com/sunluyong/node/module



### 简述导出的使用规则

````javascript
// 如果一个模块需要直接导出单个成员，而非挂载的方式(得到的就是:函数,字符串)
// 那这个时候必须使用下面这种方式
module.exports = 'hello'

//最终输出结果以这个为准,后者会覆盖前者
module.exports = function (x, y) {
  return x + y
}
````

**导出多个成员**

```javascript
module.exports = {
  add: function () {
    return x + y
  },
  str: 'hello'
}
```