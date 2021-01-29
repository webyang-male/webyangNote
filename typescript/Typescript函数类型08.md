---
title: Typescript函数类型
date: 2020-11-18 12:52:27
tags: js/typescript
categories: Javascript
---

### 函数类型

我们可以给每个参数添加类型之后再为函数本身添加返回值类型。
		Typescript能够根据返回语句自动推断出返回值类型，因此返回值的类型一般可以忽略,写上去的目的是为了增加可读性，可以很方便的知道函数的参数和结果的值的类型.

<!-- more -->

```javascript
//常规函数
let x = 13;
function addLove(y, z) {
    return x + y + z;
}
console.log(addLove("14","520")+"婷宝儿");
```

```typescript
function add(x: number, y: number) : number {
    //typescript参数定义类型后的函数
    //x ,y参数的值限制为数字，函数的返回值限制为数字
    return x + y;
}
```

------

##### 函数的完整类型定义

```typescript
let myAdd: (value1: number, vaLue2: number) => number = function (x: number, y: number): number { return x + y; }
```

***上面的代码分两部分:***

1．第一部分是变量myAdd的类型定义部分，该部分规定了myAdd的值是一个函数，函数有两参数x , y ,值都得是数字，函数的执行返回的结果得是一个数字

2．第二部分是创造了一个实际的函数，并把这个函数赋值给了变量myAdd

✍函数类型包含两部分:参数类型和返回值类型。当写出完整函数类型的时候，这两部分都是需要的。

我们以参数列表的形式写出参数类型，为每个参数指定一个名字和类型,只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为void而不能留空。

------

### 可选参数和默认参数

TypesScript里的每个函数参数都是必须的。这不是指不能传递null或undefined作为参数，而是说编译器检查用户是否为每个参数都传入了值。编译器还会假设只有这些参数会被传递进函数。

<font style="color:pink;font-size:18px;">简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致</font>

```typescript
function buidName(firstName:string,lastName:string){
    return firstName + "-"+lastName;
}

let result = buidName("Bob");//error,少了一个参数
let result2 = buidName("Bob","Mike","John");//error,多了一个参数
let result3 = buidName("Bob","婷宝儿");//正确,刚刚好
```

##### 可选参数

javaScript里，每个参数都是可选的，可传可不传。没传参的时候，它的值就是<font style="color:tomato;">undefined</font>.在Typescript里我们可以在参数名旁使用?实现可选参数的功能。

```typescript
function buidName(firstName:string,lastName?:string){
    return firstName + "-"+lastName;
}

let result = buidName("Bob");//通过,第二个参数可以不写
let result2 = buidName("Bob","Mike","John");//error,多了一个参数
let result3 = buidName("Bob","婷宝儿");//正确,刚刚好
```

可选参数必须跟在必须其他普通参数的后面

如果上例我们想让first name是可选的，那么就必须调整它们的位置，把first name放在后面。

##### 默认参数

在Typescript里，我们也可以为参数提供一个默认值．当用户没有传递这个参数或传递的值是undefined时。它们叫做有默认初始化值的参数．默认参数的写法与ES6一致

```typescript
function buildName(firstName: string, LastName?: string) {
    //可选参数函数
}
function buildName1(firstName: string, LastName = "tingbao") {
    //默认参数函数
}
```

与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。如果带默认值的参数出现在必须参数前面，用户必须明确的传入undefined值来获得默认值。

------

### 剩余参数和this

必要参数，默认参数和可选参数有个共同点:它们表示某一个参数。有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。在Javascript里，你可以使用arguments来访问所有传入的参数。

##### …rest

在Typescript里，你可以把所有参数收集到一个变量里:和ES6的rest一个道理.

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    //fristName是一个字符串类型的值，剩余的参数必须也得是字符串,然后统一放在一个名为restOfNamede数组里面
    return firstName + "" + restOfName.join(" ");
}
let employeeName = buildName("tingbao", "dazhao", "MIKE", "TRUMP");
```

剩余参数会被当做个数不限的可选参数。可以一个都没有，同样也可以有任意个。编译器创建参数数组，名字是你在省略号（...）后面给定的名字，你可以在函数体内使用这个数组。

##### this

在javascript里,this的值有两大类确定逻辑:

1. 在普通函数内, this的值指的就是调用该函数的对象

2. 在箭头函数内, this的值指的就是定义该函数时所在的环境对象

   **Typescript沿袭了ES6的设计核心**

   ```typescript
   let deck = {
       suits: ["hearts", "spades", "clubs", "diamonds"],
       cards: Array(52),
       createCardPicker: function () {
           return () => {
               let pickedCard = Math.floor(Math.random() * 12);
               let pickedSuit = Math.floor(Math.random() / 6);
               return{suit:this.suits[pickedSuit],card:pickedCard % 13};
               //此处的this指向的是deck
           }
       }
   }
   let cardPicker  = deck.createCardPicker();
   let pickedCard = cardPicker();
   
   alert("card:"+pickedCard.card+"of"+pickedCard.suit)
   ```

------

### 重载

##### 普通重载

JavaScript本身是个动态语言。Javascript里函数根据传入不同的参数而返回不同类型的数据是很常见的。

```javascript
let suits = ["hearts", "spades", "clubs", "diamonds"];
function pickcard(x): any {
    //函数的返回值可以是任意类型
    if (typeof x == "object") {//如果值的类型是对象
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    else if(typeof x == "number"){
        //如果值的类型是数字
        let pickedSuit= Math.floor(x / 13);
        return {suit:suits[pickedSuit],card:x % 13};
    }
}
let my =[{suit:"diamonds",card:2},{suit:"panda",card:10},{suit:"hearts",card:99999}];
let pickCard1 = my[pickcard(my)];
console.log("card: "+pickCard1.card+" of:"+pickCard1.suit);//card: 10 of:panda

let pickCard2 = pickcard(100);
console.log("card: "+pickCard2.card+" of:"+pickCard2.suit);//card: 9 of:undefined

```

pickcard方法根据传入参数的不同会返回两种不同的类型。如果传入的是代表纸牌的对象，函数作用是从中抓一张牌。如果用户想抓牌，我们告诉他抓到了什么牌.

##### TypeScript基于类型系统的函数重载实现

方法是为同一个函数提供多个函数类型定义来进行函数重载。编译器会根据这个列表去处理函数的调用。下面我们来重载pickcard函数。

```typescript
let suits = ["hearts", "spades", "clubs", "diamonds"];

//新加入2行
function pickcard(x:{suit:string;card:number;}[]):number;
//如果传入函数的函数格式是一个数组,且每一个数组都是一个里面有suit和card属性对象时,返回值是数字
function pickcard(x:number):{suit:string;card:number}
//如果传入函数的参数格式是一个数字,则返回一个对象，对象里面有suit和card属性

function pickcard(x): any {
    //函数的返回值可以是任意类型
    if (typeof x == "object") {//如果值的类型是对象
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    else if(typeof x == "number"){
        //如果值的类型是数字
        let pickedSuit= Math.floor(x / 13);
        return {suit:suits[pickedSuit],card:x % 13};
    }
}
let my =[{suit:"diamonds",card:2},{suit:"panda",card:10},{suit:"hearts",card:99999}];

let pickCard1 = my[pickcard(my)];
console.log("card: "+pickCard1.card+" of:"+pickCard1.suit);//card: 10 of:panda

let pickCard2 = pickcard(100);
console.log("card: "+pickCard2.card+" of:"+pickCard2.suit);//card: 9 of:undefined

```

简单来说就是通过多重类型定义来预设多种允许的参数和返回值，从而实现重载的类型判断

<font style="color:hotpink;font-size:18px;">注意</font>

⚠️function pickcard(x):any并不是重载列表的一部分，因此这里只有两个重载:一个是接收对象另一个接收数字。以其它参数调用pickcard会产生错误。