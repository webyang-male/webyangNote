---
title: Node-studyDay8
date: 2020-12-24 09:30:31
tags: Node
categories: Node
description: require方法加载规则
---

**所需代码文件:**

`foo.js`

```js
console.log('foo 文件模块被加载了')
```

`main.js`

```js
var fn = require('./foo')
console.log(fn);
```

 如果是非路径形式的模块标识

`main.js`

```javascript
require('foo.js')//报错
```

这时,Node不会把它(foo.js)当做路径,可能会把它认作核心模块或者第三方模块

```nginx

// 路径形式的模块：
//  ./ 当前目录，不可省略
//  ../ 上一级目录，不可省略
//  /xxx 几乎不用
//  d:/a/foo.js 几乎不用
//  首位的 / 在这里表示的是当前文件模块所属磁盘根路径
//  .js 后缀名可以省略
// require('./foo.js')
```

核心模块的本质也是文件
	   核心模块文件已经被编译到了二进制文件中了，我们只需要按照名字来加载就可以了

```javascript
//例:
require('fs')
require('http')
```

#### **第三方模块**

**文件目录:**

```
├── demoProjject
│ ├── node_modules(art-template下载依赖包)
│ ├── main.js
│ ├── foo.js
```

<font style="color:skyblue;">凡是第三方模块都必须通过 npm 来下载</font>
使用的时候就可以通过 require('包名') 的方式来进行加载才可以使用
不可能有任何一个第三方包和核心模块的名字是一样的
既不是核心模块、也不是路径形式的模块
先找到当前文件所处目录中的 `node_modules` 目录

```nginx

   node_modules/art-template
   node_modules/art-template/package.json 文件
   node_modules/art-template/package.json 文件中的 main 属性
  
```

​    main 属性中就记录了 art-template 的入口模块
   		然后加载使用这个第三方包
   		实际上最终加载的还是文件

------

  如果 package.json 文件不存在或者 main 指定的入口模块是也没有
 		则 node 会自动找该目录下的 index.js
		 <font style="color:tomato;">也就是说 index.js 会作为一个默认备选项</font>

举个🌰:

我们首先在 `node_modules 目录中建立一个名为`a`的文件夹/目录

在目录a下建立`foo.js` `index.js` `package.json` `main.js`4个文件

`foo.js`

```js
console.log('目录a中的foo 文件模块被加载了')
```

`package.json`

```js
{
	//这里先不写
}
```

`index.js`

```javascript
console.log('目录a中的index.js文件模块被加载了')
```

`main.js`(node运行文件)

```js
require('a')
```

**场景1**

`package.json`

```js
{
	//这里先不写
}
```

```js
//输出
目录a中的index.js文件模块被加载了
```

**场景2**

`package.json`

```js
{
	"main":"foo.js"
}
```

```js
//输出
目录a中的foo 文件模块被加载了
```

**场景3**

`package.json`直接删掉,不存在了

```js
//输出
目录a中的index.js文件模块被加载了
```

如果以上所有任何一个条件都不成立，则会进入上一级目录中的` node_modules `目录查找

如果上一级还没有，则继续往上上一级查找
	   如果直到当前磁盘根目录还找不到，最后报错：

```nginx
can not find module xxx
```

⚠️我们一个项目有且只有一个 `node_modules`，放在项目根目录中，这样的话项目中所有的子目录中的代码都可以加载到第三方包
	   不会出现有多个 `node_modules`

```nginx
// 模块查找机制
//    优先从缓存加载
//    核心模块
//    路径形式的文件模块
//    第三方模块
//      node_modules/art-template/
//      node_modules/art-template/package.json
//      node_modules/art-template/package.json main
//      index.js 备选项
//      进入上一级目录找 node_modules
//      按照这个规则依次往上找，直到磁盘根目录还找不到，最后报错：Can not find moudle xxx
//    一个项目有且仅有一个 node_modules 而且是存放到项目的根目录
```

[![art-templatec640e0e08e913a29.md.png](https://cdn.longdoer.com/2020/12/24/art-templatec640e0e08e913a29.png)](http://www.ipicbed.com/image/sp3sg)