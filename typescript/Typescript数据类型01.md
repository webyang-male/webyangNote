---
title: Typescript数据类型01
date: 2020-11-11 14:03:51
tags: js/typescript
categories: Javascript
---

**typescript是一门编译型语言,微软创造.**

### 1.typescript环境配置

#### 1.1 Node安装

#### 1.2   在安装好node之后，在命令行工具中输入npm install -g typescript

⚠️ 记得在打开命令行工具时，右键管理员打开

![ts.png](https://i.loli.net/2020/11/18/BVtbM9FydAojpJm.png)

<!-- more -->

#### 1.3  检查安装成功与否:

cmd输入`tsc`,会出现version版本号和一大片命令提示,即表示安装成功.

😊TypeScript 是 JavaScript 的超集，`.js` 文件可以直接重命名为 `.ts` 即可

### 2.typescript编译

> 语法: tsc 文件名

eg:新建一个ts文件  `demo.ts`

终端输入 `tsc demo.ts`即可编译成js文件

✍因为Node环境安装完毕,就不需要去客户端运行js了,直接编译器本地运行js,命令`node 文件名`

<font  style="color:red;">如果编译完成后,ts文件变量等出现报错,删掉编译好的js文件所有代码即可,因为typescript会检测全局代码</font>

### 3.基础变量声明

#### 3.1 let[变量]:[类型]=值;

这种方式实现的变量命名有一个好处，那就是赋值语句中等号右侧值的类型和等号左侧自行定义的值的类型必须得是完全一致的,否则会报错.

如果赋值的时候传入的是正确的数值,后期又重新赋值了错误类型的值,同样也会报错

简单理解:`数据类型,从一而终`

```typescript
//不能将string类型分配给number类型
//例1
let a:number = 18;
a = '婷宝儿'
//例2
let a:number = "18";
a = "20"
```

⚠️未设置变量类型,变量类型将被强行依据变量初始值类型定义

```typescript
//不能将string类型分配给number类型
let a = 18;
a="18"
```

#### 3.2  

let[变量]:[类型];

如果只是创造了变量并规定了类型，那么这个变量默认的值就是`undefined`

如果只是写了一条没有确定的值的变量声明语句，那么这个值用起来的时候就是undefined,但是一旦后面有其他的新的赋值操作，还是会按照变量的`预设格式`来的

```typescript
let a:number;
if (a == undefined) {
    //代码会执行,控制台输出语句
    console.log("婷宝儿真漂亮!")
}
```

```typescript
let a:number;
console.log(a);//undefined

a = "18";//错误,a的变量类型是number类型

a = 18;//正确
```

### 4.Typescript的数据类型之enum和元祖类型

#### 4.1元祖类型Tunple

元组类型表示一个已知元素数量和类型的数组，各元素的类型不必相同

👍TypeScript有一个优点在于它在编译的时候会把代码中所有的代码错误都找出来,而不是像avaScript—处报错就会停止解析代码

```typescript
//数组a被定义死了数组长度和对应元素数据类型
let a:[string,number,number,object,boolean];//初始值类型

//不能将类型“number”分配给类型“[string，number，number，object,boolean]”
a = 1;//报错

a = ["1"];//源具有1 个元素，但目标需要5个。

//源具有6个元素,但目标仅允许5个。
a = ["婷宝儿",1314,520,{boyfriend:"zzy"},true,99]

//数据类型,长度一一匹配,正确
a = ["婷宝儿",1314,520,{boyfriend:"zzy"},true]

```

#### 4.2枚举类型enum

##### enum类型是对JavaScript标准数据类型的一个补充。像C#等其它语言一样，使用枚举类型可以为一组数值赋予变量名称。

```typescript
enum Color{x,y,z};
console.log(Color);//{ '0': 'x', '1': 'y', '2': 'z', x: 0, y: 1, z: 2 }
console.log(Color.x);//0
```

编译后:

```javascript
var Color;
(function (Color) {
    Color[Color["x"] = 0] = "x";
    Color[Color["y"] = 1] = "y";
    Color[Color["z"] = 2] = "z";
})(Color || (Color = {}));
;
console.log(Color);//{ '0': 'x', '1': 'y', '2': 'z', x: 0, y: 1, z: 2 }
console.log(Color.x);//0
```

默认情况下，从0开始为元素编号。你也可以手动的指定成员的数值

如果只给第一个设置编号,那么这个将成为起始编号,如果每个都设置,<font style="color:tomato;">相当于手动设置每个元素的下标</font>

```typescript
enum Color{x=1,y,z};
console.log(Color.x,Color.y,Color.z);
```

编译后:

```javascript
var Color;
(function (Color) {
    Color[Color["x"] = 1] = "x";
    Color[Color["y"] = 2] = "y";
    Color[Color["z"] = 3] = "z";
})(Color || (Color = {}));
;
console.log(Color.x, Color.y, Color.z);//1,2,3
```

如果改变中间值呐?

```typescript
enum Color{x,y=9,z};

console.log(Color.x,Color.y,Color.z);

```

编译后:

```javascript
var Color;
(function (Color) {
    Color[Color["x"] = 0] = "x";
    Color[Color["y"] = 9] = "y";
    Color[Color["z"] = 10] = "z";
})(Color || (Color = {}));

console.log(Color.x, Color.y, Color.z);//0,9,10
```

> ##### enum的值里面不能设置为对象,或是利用变量间接引用对象的值

```typescript
//错误
enum Color{x,{y=9},z};
console.log(Color);
```

```typescript
//错误
let o = {q:1};
enum Color{red = 0,o,pink};
```

> ##### 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。例如，我们知道数值为1，但是不确定它映射到color里的哪个名字，我们可以查找相应的名字:

```typescript
enum Color{red = 1,green,blue};
let colorName:string = Color[2];

console.log(colorName);//green,因为上面代码它的值为2
```

```javascript
var Color;
(function (Color) {
    Color[Color["red"] = 1] = "red";
    Color[Color["green"] = 2] = "green";
    Color[Color["blue"] = 3] = "blue";
})(Color || (Color = {}));
;
var colorName = Color[2];
console.log(colorName);
```

