---
title: Typescript接口
date: 2020-11-16 21:44:40
tags: js/typescript
categories: Javascript
---

### 一.什么是接口

TypeScript的核心原则之一是对值所具有的结构进行类型检查。它有时被称做“鸭式辨型法"或“结构性子类型化”。

在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码`定义契约`。

<!-- more -->

```typescript
{name:string}function say(name:{name:string}){

}
say();//报错,此处应有一个参数
say(1);//类型number”的参数不能赋给类型“{name: string;}”的参数。
say({name:1});//不能将类型“number”分配给类型“string”。
say({name:'婷宝儿'});//正确
```

{name:string}即所谓的接口,它对所传进来的参数作出强制约束.

#### 1.1 interface接口实现

```typescript
interface nameCheck{
    name : string;
} 
function say(name:nameCheck){

}

say({name:'婷宝儿'});//正确
say({name:1});//错误
```

代码实例2和实例1效果一致.

> 类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。

```typescript
interface nameCheck{
    name : string,
    age : number
} 
function say(name:nameCheck){

}

say({age:18,name:'婷宝儿'});//正确,参数位置调换也是可以的
```

当然多一个参数也不行的啦~

```typescript
interface nameCheck{
    name : string,
    age : number
} 
function say(name:nameCheck){

}

/* 类型"{ age: number;name: string; sex: string; }”的参数不能赋给类型nameCheck”的参数。
对象文字可以只指定已知属性，并且“sex”不在类型"nameCheck”中。 */
say({age:18,name:'婷宝儿',sex:"女"});//报错

//这种也别想通过哈哈哈哈
say({age:18,name:'婷宝儿'},{age:20,name:'zzy'});//报错--应有1个参数,但获得2个
```

然后…就这?  既然严格,那就贯彻到底咯💪

```typescript
interface nameCheck{
    name : string,
    age : number
} 
function say(name){
//未作检查
}

say({age:18,name:'婷宝儿'},{age:20,name:'zzy'});//报错--应有1个参数,但获得2个

```

想要通过? 可以! 加一个args参数,自己动

```typescript
interface nameCheck{
    name : string,
    age : number
} 
function say(name:nameCheck,...args){

}

say({age:18,name:'婷宝儿'},{age:20,name:'zzy'});//正确
```

### 二.interface接口特性

#### 2.1interface接口实现之可选属性

很多框架的API接口里面都给我们提供了一些可选的属性,用户可以选择是否输入该属性信息,无论输入或是不输入都不会报错

可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。

```typescript
interface nameCheck{
    name : string,
    age ?: number//可选参数后面加上一个----?,需在冒号前
} 
function say(name:nameCheck,...args){

}

say({name:'婷宝儿'});//正确
//say({age:18,name:'婷宝儿'});//同样正确,因为参数可选
```

```typescript
interface nameCheck{
    name : string,
    age ?: 21
} 
function say(name:nameCheck,...args){

}
//值不同
say({age:18,name:'婷宝儿'});//报错 不能将类型“18”分配给类型“21”.
```

##### 😃interface接口实现之可选属性的优点

可选属性的好处:

​		一是可以对可能存在的属性进行预定义

​		二是可以捕获引用了不存在的属性时的错误

```typescript
interface nameCheck{
    name : string,
    age : number,
    sex ?: string
} 
function say(famale:nameCheck){
    //类型“nameCheck”上不存在属性“address”。
    console.log(famale.address,famale.name,famale.age);
    
}

say({age:18,name:'婷宝儿',sex:"女"});

```

当使用了可选属性之后，那么对于这传入的对象里面的属性的引用就会被限制，如果引用了接口里未定义的属性时,就会报错

```typescript
function say(famale) {
    //类型“nameCheck”上不存在属性“address”。
    console.log(famale.address, famale.name, famale.age);

}

let msg = { age: 18, name: '婷宝儿', sex: "女" };
say(msg);
console.log(say);//undefined 婷宝儿 18

```

如果没有使用接口的话,那么引用不存在的属性时,是不会报错的

------

#### 2.2interface接口实现之只读属性

接口不仅可以用在函数的传参上,也可以用作其他语句.比如,一些对象属性只能在对象刚刚创建的时候修改其值。

你可以在属性名前用readonly来指定只读属性:

```typescript
interface Point {
    readonly x: number,
    readonly y: number
}

let p1: Point = { x: 6, y: 8 }

p1.x = 10;//无法分配到“x”，因为它是只读属性。
```

最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。做为变量使用的话用const，若做为属性则使用readonly。

#### 2.3interface接口实现之属性检查

在可选属性一页中咱们讲过，当我们给一个对象设置了可选属性是，那么函数内部对于其他的未在接口内定义的参数进行调用就会直接报错

不过,绕开这些检查也不是不可以

> 最简便的方法是使用类型断言
>
> 最佳的方式是在接口里添加一个字符串索引签名

以下是 **字符串索引签名案例**

```typescript
interface nameCheck {
    age: number,
    [property: string]: number//规定传进来的属性都是string,值都是number
}
function say(famale: nameCheck) {

}

say({ age: 18, "id": 7, });//正确
say({ age: 18, id: 7, });//正确

//name参数报错 不能将类型“string”分配给类型“number”
//[property: string]: number//规定传进来的属性都是string,值都是number
say({ age: 18, id: 7,name:"婷宝儿" });

```

⚠️在进行对其他值的筛选时,一定要注意是在不违反之前的规则前提下进行

```typescript
interface nameCheck {
    name: string,//值要求是string类型
    age: number,
    [property: string]: number//规定传进来的属性都是string,值要求是number类型
}
function say(famale: nameCheck) {

}
//nameCheck接口参数第1个和第3个冲突
say({ age: 18, id: 7, name: "婷宝儿" });

```

想让上面成立,改一下数据类型即可

```typescript
interface nameCheck {
    name: string,
    age: number,
    [property: string]: any//对传进来的属性值没有约束
}
function say(famale: nameCheck) {

}
//正确
say({ age: 18, id: 7, name: "婷宝儿" });

```

#### 2.4interface接口实现之函数描述

接口能够描述JavaScript中对象拥有的各种各样的特性。除了描述带有属性的普通对象外，接口也可以描述函数类型。

```typescript
interface checkfn{
    //圆括号内是参数的要求，冒号后面是返回值的类型要求
    (m:string,n:number):string;
}

//对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。
let fn:checkfn = function(a,b){
  //let len = b.length;//报错,number类型没有length属性方法
    return a + b;
}

fn("9",9)
```

<font  style="color:red;font-size:18px;">typescript在编译的时候，会对函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。</font>

如果你不想指定每个参数的特定类型也可以不写,TypeScript的类型系统会自动根据接口推断出参数类型，函数的返回值类型是通过其返回值推断出来的．如果让这个函数返回数字或字符串，类型检查器会警告我们函数的返回值类型与接口中的定义到底匹不匹配

#### 2.5interface接口实现之索引类型

与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如x[10]或myName[ "Tom"]

```typescript
interface proCheck {
    [index: string]: string;
    readonly [index: number]: string;
}

let obj: proCheck = {
    "name": "婷宝儿",
    996: "No996"
};

console.log(obj[996]);//No996
//obj[996] = "995";//类型“proCheck”中的索引签名仅允许读取

```

> TypeScript支持两种索引签名:***字符串和数字***。可以同时使用两种类型的索引，**但是数字索引的返回值必须是字符串索引返回值类型的子类型。**
>
> 这是因为当使用number来索引时，JavaScript会将它转换成string然后再去索引对象。也就是说用100 (一个number)去索引等同于使用"100”(一个string)去索引，因此两者需要保持一致。

```typescript
interface proCheck {
    [index: string]: number;
    length: number;
    name: string;
}
//错误，`name`索引本就是字符串，和上面的index设定是一样的
//但是上面已经规定了索引值是字符串的,值必须是number
```

> 可以将索引签名设置为**只读**，这样就防止了给某些特定的索引进行赋值

```typescript
interface ReadonlyArray {
    readonly [index: number]: string;
    [index: string]: string;
}
let newArray: ReadonlyArray = { 0: "婷宝", 1: 'zzy', "age": "18" }
newArray[0] = "婷宝儿";//错误,索引为数字的键值对定义了readonly,不允许修改值
newArray.age = "20" //正确,索引为字符串的键值对可以修改值

```

#### 2.6interface接口实现之“类class”类型

实现类接口的关键字是implements(英文释义为:使生效,执行)

利用类接口的设计可以规范出一个标准类最起码所具备的属性和方法

```typescript
interface classCheck {
    currentTime: Date;
    setTime(d:Date);

}

class Clock implements classCheck {
   currentTime:Date;
   setTime(d:Date){
    this.currentTime = d;
   }
   constructor(x:number,y:number){}
}
```

#### 2.7interface接口实现之继承接口

和类一样，接口也可以相互继承。这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape{
    SquareLength : number;
}

let Producton : Square = {
    //错误,所需类型来自属性"color"，在此处的“Square”类
    color : 99 ,//color值必须是"string"类型
    SquareLength : 520
}

```



```typescript
interface Shape {
    color: string;
}

interface Square extends Shape{
    SquareLength : number;
}

interface Producton extends Square,Shape{
    productonLength : 520
}

```

一个接口也可以继承多个接口，创建出多个接口的合成接口，上面的代码运行结果是，一个调用了Square接口的对象里必须有color , penwidth , sideLength属性

#### 2.8 interface接口实现之混合类型

```typescript
function getCounter():Counter{
  //<Counter>类型断言
    let counter = <Counter>function(start:number){
        return "婷宝儿"
    };
    counter.interval = 52099;
    counter.reset = function (){}
    return counter;
}

let count = getCounter();
count(10);
count.reset();
count.interval = 20;
```

函数也是对象，所以我们可以给一个函数制定一个接口，定义函数的传参和自定义的额外属性