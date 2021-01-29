---
title: Typescript之lib.d.ts
date: 2020-12-18 14:42:47
description: 😋typescript学习完结!
tags: js/typescript
categories: Javascript
---

### 什么是lib.d.ts

当你安装Typescript时，会顺带安装lib.d.ts等声明文件。此文件包含了avascript运行时以及DOM中存在各种常见的环境声明。

- 它自动包含在和ypescript项目的编译上下文中
- 它能让你快速并始书写经过类型检查的Javascript代码

你可以通过指定`--noLib `的编译器命令行标志（或者在`tsconfig.json`中指定选项`noLib: true，`编译选项)从上下文中排除此文件(不过不建议)。

```typescript
tsc --noLib
```

`test.ts`

```typescript
let prop=123;
let s=prop.toString();
```

这段代码的类型检查正常，因为`lib.d.ts` 为所有Javascript对象定义了`toString`方法。

### lib.d.ts的内部

`lib.d.ts`的内容主要是一些变量声明(如: window、document、math)和一些类似的接口声明(如: window、Document、Math)。

最简单的方式寻找代码的类型(如: Math.floor）)是使用IDE的 F12键跳转到定义(使用方式是,把鼠标放在对应的方法代码上，再按下f12键)

![lib53cdf8bf9a6f648f.png](https://cdn.longdoer.com/2020/12/19/lib53cdf8bf9a6f648f.png)

### 修改lib.d.ts之全局window

在TypeScript 中，接口是开放式的，这意味着当你想使用不存在的成员时，你仅仅是需要添加它们至` lib.d.ts `中的接口声明中，Typescript将会自动接收它。

注意，你需要在全局模块中做这些修改，以使这些接口与`lib.d.ts `相关联。建议在项目文件夹中创建一个称为 globals.d.ts 的特殊文件,这样既可修改全局也不会影响其他的项目代码

`globle.d.ts`

```typescript
interface Window {
    hello(): void;
}
```

### 修改lib.d.ts之全局Math

当你想在Math全局变量上添加你需要的属性时，你仅需要把它添加至Math 的全局接口上即可

![math_tsc57af5ba0b651c81.png](https://cdn.longdoer.com/2020/12/19/math_tsc57af5ba0b651c81.png)

![ts_libbf4e5172e9640fa8.png](https://cdn.longdoer.com/2020/12/19/ts_libbf4e5172e9640fa8.png)

### 修改lib.d.ts之全局Date

接口`DateConstructor `(即Date的数据格式)与你在上文中看到的 Math 和 window 接口一样，因为它涵盖了可以使用的 Date 全局变量的成员(如: Date.now())。除此之外，它还包含了可以让你创建Date 实例的构造函数签名(如: new Date())

![tslib_date1a3ed214dbad91ff.png](https://cdn.longdoer.com/2020/12/19/tslib_date1a3ed214dbad91ff.png)

### 修改lib.d.ts之全局string

在`lib.d.ts `里string，与Date类似(全局变量 string，StringConstructor接口，String接口)。但是值得注意的是，string 接口也会影响字符串字面量

![lib_ts_stringb3296083bbe44edb.png](https://cdn.longdoer.com/2020/12/19/lib_ts_stringb3296083bbe44edb.png)

### 自定义lib.d.ts

如上文说提及，使用`--noLib `编译选项会导致 Typescript 排除自动包含的 `lib.d.ts `文件。那为啥需要实现这种功能呢?

- 运行的Javascript环境与基于标准浏览器运行时环境有很大不同;
- 希望在代码里严格的控制全局变量，例如: lib.d.ts定义了item作为全局变量，你并不希望它泄漏到你的代码里。

一旦你排除了默认的` lib.d.ts` 文件，你可以在编译上下文中包含一个类似命名的文件，Typescript将选择它进行类型检查。

⚠️小心使用`--noLib`选项，一旦你使用使用了它，当你把你的项目分享给其他人时，它们也将被迫使用`--noLib`选项，更糟糕的是，如果将这些代码放入你的项目中，你可能需要将它放入你的基于`lib `代码中。



### --lib选项

设置编译目标为es6时，能导致lib.d.ts包含更多的像Promise 的现代(es6）内容的环境声明.一些时候，你想要解耦编译目标（生成的JavaScript版本）和环境库支持之间的关系。例如对于Promise，你的编译目标是`--target es5`，但是你仍然想使用它，这个时候，你可以使用`lib` 对它进行控制。

```typescript
>tsc --target es5 --lib dom,es6
```

#### 命令行

<font style="color:skyblue;">你可以通过命令行或者在tsconfig.json中提供此选项(推荐)</font>

`config.json`

![tsconfig_lib2dc84c242b6e84fd.png](https://cdn.longdoer.com/2020/12/19/tsconfig_lib2dc84c242b6e84fd.png)



### 

![lib-1679cef2b11d760e9.png](https://cdn.longdoer.com/2020/12/19/lib-1679cef2b11d760e9.png)