---
title: Node-studyDay7
date: 2020-12-24 8:54:50
tags: Node
categories: Node
description: 简窥exports/module-expots和缓存加载
---

#### exports和module-expots的区别

在 Node 中，每个模块内部都有一个自己的 module 对象
	   该 module 对象中，有一个成员叫：`exports` 也是一个对象
	   也就是说如果你需要对外导出成员，只需要把导出的成员挂载到 `module.exports `中

```javascript
// var module = {
//   exports: {
//     ....
//   }
// }

module.exports.foo = 'bar'

module.exports.add = function (x, y) {
   return x + y
}
// 谁来 require 我，谁就得到 module.exports
// 默认在代码的最后有一句：
// 一定要记住，最后 return 的是 module.exports
// 不是 exports
// 所以你给 exports 重新赋值不管用，
// return module.exports
```

我们发现，每次导出接口成员的时候都通过` module.exports.xxx = xxx` 的方式很麻烦，点儿的太多了
所以，Node 为了简化你的操作，专门提供了一个变量：exports 等于 `module.exports`

```javascript
// 两者一致，那就说明，我可以使用任意一方来导出内部成员
// console.log(exports === module.exports)//=>true
```

那么上面代码等价于

```javascript
exports.foo = 'bar'
module.exports.add = function (x, y) {
  return x + y
}
```

当一个模块需要导出单个成员的时候
	   直接给 exports 赋值是不管用的

````js

exports.a = 123

exports = {}
exports.foo = 'bar'

module.exports.b = 456
//导出结果{a:123,b:456}
````

```js
//给 exports 赋值会断开和 module.exports 之间的引用
//同理，给 module.exports 重新赋值也会断开

//这里导致 exports !== module.exports
module.exports = {
  foo: 'bar'
}

// 但是这里又重新建立两者的引用关系
exports = module.exports

exports.foo = 'hello'
//这时候输出 {foo: bar}

exports.foo = 'bar'

// {foo: bar, a: 123}
module.exports.a = 123

// exports !== module.exports
// 最终 return 的是 module.exports
// 所以无论你 exports 中的成员是什么都没用
exports = {
  a: 456
}

// {foo: 'haha', a: 123}
module.exports.foo = 'haha'

// 没关系，混淆你的
exports.c = 456

// 重新建立了和 module.exports 之间的引用关系了
exports = module.exports

// 由于在上面建立了引用关系，所以这里是生效的
// {foo: 'haha', a: 789}
exports.a = 789

// 前面再牛逼，在这里都全部推翻了，重新赋值
// 最终得到的是 Function
module.exports = function () {
  console.log('hello')
}


// 真正去使用的时候：
//    导出多个成员：exports.xxx = xxx
//    导出多个成员也可以：module.exports = {
//                        }
//    导出单个成员：module.exports
```

 如果你实在分不清楚 exports 和 module.exports
		你可以选择忘记 exports
		而只使用 module.exports 也没问题

```nginx
module.exports.xxx = xxx
moudle.exports = {}
```

#### node优先从缓存加载

代码🌰

`main.js`

```nginx
require('./a')

// 优先从缓存加载
// 由于 在 a 中已经加载过 b 了
// 所以这里不会重复加载
// 可以拿到其中的接口对象，但是不会重复执行里面的代码
// 这样做的目的是为了避免重复加载，提高模块加载效率
var fn = require('./b')

console.log(fn)

```

`a.js`

```nginx
console.log('a.js 被加载了')
var fn = require('./b')

console.log(fn)
```

`b.js`

```nginx
console.log('b.js 被加载了')

module.exports = function () {
  console.log('hello bbb')
}

```

输出:

```nginx
a.js 被加载了
b.js 被加载了
[Function (anonymous)]
[Function (anonymous)]
```

