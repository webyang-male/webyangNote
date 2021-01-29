---
title: Typescript数据类型02
date: 2020-11-12 09:04:36
tags: js/typescript
categories: Javascript
---

### <center>Typescript数据类型</center>

#### 1.1  任意类型any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。这些值可能来自于动态的内容，比如来自用户输入或是从后端请求来的数据.

这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。那么我们可以使用any类型来标记这些变量.

<!-- more -->

```typescript
//变量“a”隐式具有“any”类型，但可以从用法中推断出更好的类型

let a;
a = 1;//a 的类型是any

let a = null;
a = 1;//any

let a = undefined;
a = 1;//any

```

```typescript
//其声明类型不为“void”或“any”的函数必须返回值
function Name(name:string):number{
//此处括号后定义了返回值为---number
 //此时返回值为undefined
}

function Name(name:string):number{
    return 18;//正确
}
-----------------------------------------
function Name(name:string):any{
    return 18;//正确
}

function Name(name:string):any{
    return "18";//正确
}

function Name(name:string):any{
   
}

....
```

#### 1.2 无类型 void

*Void表示没有任何类型*

你只能为它赋予`undefined`和`null`

```typescript
let tingbao: void = 18;//不能将类型“number”分配给类型“void”。

let obj: void = "婷宝儿";//不能将类型“string”分配给类型“void”。

let obj: void = null;//正确
```

#### 1.3 undefined/null

默认情况下null和undefined是**所有类型**的<font style="color:skyblue;">子类型</font>

例如:可以把null和undefined赋值给number类型的变量

```typescript
//以下都是正确的
let tingbao: string = null;

let tingbao1: string = undefined;

let tingbao2: number = null;

let tingbao3: boolean = null;

...

let tingbao4: null = null;

let tingbao5: undefined = undefined;

let tingbao6: undefined = null;

let tingbao7: null = undefined;
```

#### 1.4 Nerver类型

never类型表示的是那些永不存在的值的类型

✍这个严格来说算不上啥新的数据类型，只是开发者对于一类值所起的作用的判断而已

*例:*

​	总是会抛出异常，throw错误或是返回一个error类型的数据根本就不会有返回值的函数表达式(死循环函数)

```typescript
function noMeaning(name:string):never{
    throw new Error(name);
}

function wrong():void{
    return noMeaning("something Wrong");
};

```

```typescript
function infinity(): never {
    while(true){
        console.log("报错啦!");//无限死循环
    }
}
```

never类型是`任何类型`的子类型，也可以赋值给任何类型;

然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外)。<font style="color:red;">即使any也不可以赋值给never</font>

```typescript
let zzy:string;

let tingbao: never;

zzy = "大赵";

tingbao = "婷宝儿";//报错,never类型数据不能接受其他类型数据

//正确,一个异常抛出函数就是never类型的
tingbao = (() => {throw new Error('wrong')})();

//不报错,never类型变量可以赋值其他类型的变量
zzy = (() => {throw new Error('wrong')})();

```

