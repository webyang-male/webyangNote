---
title: Typescript数据类型03
date: 2020-11-12 09:06:15
tags: js/typescript
categories: Javascript
---

### 1.Typescript 断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。通常这会发生在你清楚地知道一个变量具有比它现有类型更确切的类型(比如说满是数字的数组,或是全都是自然数下标的对象,这只是一个举例)。

<!-- more -->

```typescript
//1.as语法
let a:any= "123";

let len:number = (a as string).length;

console.log(len);//3

//尖括号语法
let a= "婷宝儿大阔爱";

let len:number =(<string> a).length;//我们清楚知道变量a是string类型

console.log(len);//6
```

通过类型断言这种方式可以告诉编译器——“我知道自己在干什么”。

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

TypeScript会假设程序员已经进行了必须的检查。

### <font style="color:skyblue;">2.Typescript其他类型 </font>

```typescript
let x:[]= [];//数组

let s:object = [];//对象

```

✍首字母小写是类型,大写是构造函数.

