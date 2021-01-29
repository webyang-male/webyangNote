---
title: vue初识
date: 2020-12-28 08:39:15
tags: vue
categories: Vue
description: vue学习之路就此起航⛵
---

##### 一、基本的设计模式

###### 1、基本设计模式之MVC模式

[![mvc.md.png](https://s1.imagehub.cc/images/2020/12/28/mvc.png)](https://www.imagehub.cc/image/eHbiT)

###### 2、基本设计模式之MVP模式

![mvp.png](https://s1.imagehub.cc/images/2020/12/28/mvp.png)

###### 3、基本设计模式之MVVM模式

![mvvm.png](https://s1.imagehub.cc/images/2020/12/28/mvvm.png)

##### 二、SPA和MPA

```markdown
1、SPA

SPA应用：SinglePage Application应用，即单页面应用。
只有一个主页面的应用，一开始只加载一次js、css等相关资源。所有的内容都包含在主页面，对每一个功能模块组件化。单页应用跳转，就是切换相关组件，仅刷新局部资源。

2、MPA

MPA应用：MultiPage Application应用，即多页面应用。
有多个独立的页面的应用，每个页面必须重复加载js、css等相关资源。多页应用跳转，需要整页资源刷新。

3、SPA和MPA对比
```

[![spampa.png](https://s1.imagehub.cc/images/2020/12/28/spampa.png)](https://www.imagehub.cc/image/eiPE6)

##### 四、Vue的使用

###### 1、Vue的引入

可以使用cdn引入，或者是把源码下载下来然后引入。

```js
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

使用Vue开发项目，可以在谷歌浏览器中安装Vue开发工具`Vue.js devtools`(在谷歌商店安装，需要fq哦~)

###### 2、Vue的实例创建和插值

每一个Vue应用都是通过用Vue函数创建一个新的Vue实例

el：绑定的元素
	   data：绑定的数据对象
       文本插值是最基本的形式，使用双大括号`{{}}`（Mustache语法糖）
       例子中的标签`{{msg}}`将会被相应的数据对象msg属性的值替换掉，当msg的值改变时，文本中的值也会联动地发生变化。

###### 3、Vue的表达式插值

Mustache语法糖也接受表达式形式的值，表达式可以有JavaScript表达式构成。表达式是各种数值、变量、运算符的综合体。简单的表达式可以是常量或者变量的名称。表达式的值是其运算结果。

```js
// 1、JS表达式
{{msg / 100}} // 在原始值上除以100
{{true ? 1 : 0}} // 值为真,则渲染出1,否则渲染出0
{{msg.split(“ , ”)}} // 把值对应的字符串进行处理

// 2、无效示例
{{var a = 1}}  // 这是一条语句, 不是表达式
{{if (ok) { return message }}} //控制流程的代码也是没有用的
```

###### 4、Vue的计算属性:computed

模板内的表达式非常便利，但是设计他们的初衷是用于简单运算的，在模板中放入太多的逻辑会让模板过重切难以维护。所以针对这样复杂的处理逻辑，我们引入计算属性这一技术来实现。

###### 5、Vue的计算属性的setter

计算属性默认只有getter(只能读取不能设置),不过在需要时你也可以提供一个setter

###### 6、Vue的方法:methods

我们可以把同一个功能函数定义为一个方法，跟计算属性相比，计算属性是依赖缓存的，只在响应式依赖发生变化时它们才会重新求值。

###### 7、Vue的侦听属性:watch

Vue提供了一种更通用的方式--‘侦听属性’，来观察和响应Vue实例上的数据变动，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方法是最有用的。

###### 计算属性：

三种对于数据变化监听机制：
1、 computed：一个属性依赖于多个属性时，推荐使用
2、 watch()：多个属性依赖一个属性是，推荐使用
3、 Set、get：set对一个属性设置值时，会自动的调用相应的回掉函数，get的回调函数会根据，函数内部依赖的属性的改变而自动改变

##### 五、 vue原理简析

vue响应式实现的原理；
 为此网上看过相关视频和搜索相关资料，得到的简单的一句总结是： 
 通过`Object.defineProperty`去劫持`data`里的属性，将`data`全部属性替换成`getter`和`setter`，配合`发布者和订阅者模式`，每一个组件都有一个`watcher`实例，当我们对`data`属性赋值和改变，就会触发`setter`，`setter`会通知`watcher`，从而使它关联的组件进行重新渲染。

###### `Object.defineProerty`详解

###### `Object.defineProerty`的基础用法

首先这个使基于对象的方法

```js
let obj ={text:''};
Object.defineProperty(obj, 'name', {
  //value: 14,
  //writable: true,
  configurable: true,
  enumerable: true,
  set:function(val){
  	this.text=val;
  },
  get: function(){
  	return this.name;
  }
});
```

- `value` 该属性的值,可以是任何有效的 JavaScript 值（数值，对象，函数等）
- `configurable`当它值为`true`时，才能添加对象属性描述和删除，该值如果改成`false`将不可逆；
- `writable` 当它值为`true`时，才能对该属性`value`进行赋值改变
- `enumerable` 当它值为`true`时，能对该属性进行枚举
- `get` 属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值
- `set`属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。

<font style="color:skyblue;">这里vue主要用到的是属性的`getter`和`setter`方法</font>

###### ⚠️注意 `get`和`set`不能和`writable`及`value`共存，否则浏览器会报错

```shell
 Uncaught TypeError: Invalid property descriptor. Cannot both specify accessors and a value or writable attribute, # at Function.defineProperty (<anonymous>)
```



###### Object.defineProerty VS proxy

vue2.0主要是通过`Object.defineProerty`来劫持对象属性，更改`getter`和`setter`方法， vue3.0用`proxy`来替代2.0的核心功能，那么他们之间究竟有什么不同呢？

**Object.defineProperty**

- 不能监听到数组length属性的变化；
- 不能监听对象的添加；
- 只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。

**Proxy**

- 可以监听数组length属性的变化；

- 可以监听对象的添加；

- 可代理整个对象，不需要对对象进行遍历，极大提高性能；

- 多达13种的拦截远超Object.defineProperty只有get和set两种拦截。
  

  链接：https://juejin.cn/post/6872992692268990478
  来源：掘金

