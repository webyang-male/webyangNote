---
title: Typescript高级小技巧
date: 2020-11-17 13:49:40
author: 大赵菌
email: 857613794@qq.com
description: ⚠️这是一篇简洁而"不"简单的blog (°ω°)ﾉ"
tags: js/typescript
categories: Javascript
---

#### 构造函数

当我们在Typescript里声明了一个类的时候，实际上同时声明了很多东西

首先就是声明类的实例的类型。

```typescript
class Woman{
    name:string;
    constructor(message:string){
        this.name = message;
    }
    greet(){
        return "hello," + this.name;
    }
}

let obj:Woman;//声明变量obj的类型为Woman
obj = new Woman("婷宝儿");//创造一个Woman实例
console.log(obj.greet());//输出 hello,婷宝儿
```

实例部分和静态部分是分离开的

```typescript
class Greeter {
    static standardGreeting = "Hello， there";
    greeting: string;
    greet() {
        if (this.greeting) {
            return "Hello," + this.greeting;
        }
        else {
            return Greeter.standardGreeting;
        }
    }
}
let greeter1: Greeter;
greeter1 = new Greeter();
console.log(greeter1.greet());//hello , there
let greeterMaker: typeof Greeter = Greeter;
//这里的typeof Greeter的语法意思是是取Greeter类的类型，而不是实例的类型
greeterMaker.standardGreeting ="Hey there!";
let greeter2: Greeter = new greeterMaker();
console. log(greeter2.greet());//"Hey there!";
```

#### 把类当做接口使用

因为类可以创建出类型，所以你能够在允许使用接口的地方使用类。

```typescript
class Something{
    x:string;
    y:number
}
interface Human extends Something{
    z:number;
}

let human:Human = {x:"婷宝儿",y:18,z:99}
```