---
title: Typescript高级类型02
date: 2020-12-02 22:29:32
tags: js/typescript
categories: Javascript
---

## 字符串字面量类型

字符串字面量类型允许你指定字符串必须的固定值。
	   在实际应用中，字符串字面量类型可以与联合类型，类型保护和类型别名很好的配合。通过结合使用这些特性，你可以实现类似枚举类型的字符串。

<!-- more -->

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";
class UIElement {
    animate(dx:number,dy:number,easing:Easing){
        if (easing === "ease-in") {
            //...
        } else if(easing === "ease-out"){
            
        }else if (easing === "ease-in-out") {
            
        }else{
            //err
        }
    }
}
let button = new UIElement();
button.animate(0,0,"ease-in");
button.animate(0,0,"ease-in-xxx");//类型"ease-in-xxx"”的参数不能赋给类型“Easing”的参数。

```

<font style="color:tomato;">**字符串字面量类型还可以用于区分函数重载**</font>

```typescript
 function creatElement(tagName:"img"):HTMLImageElement;
 function creatElement(tagName:"input"):HTMLInputElement;

 //...more overloads...
 function creatElement(tagName:string):Element{
     //...code goes here...
     return;
 }
```

## 数字字面量类型

TypeScript还具有数字字面量类型。

```typescript
function sort(): 1 |2 |3 | 4 |5 | 6 {
  return;
}
```

## 可辨识联合(Discriminated Unions)

你可以合并单例类型，联合类型，类型保护和类型别名来创建一个叫做可辨识联合的高级模式，它也称做标签联合或代数数据类型。可辨识联合在函数式编程很有用处。一些语言会自动地为你辨识联合;而TypeScript则基于已有的JavaScript模式。

它具有3个要素:

- ​	具有普通的单例类型属性 —— 可辨识的特征。
- ​    一个类型别名包含了那些类型的联合—— 联合。
- ​    此属性上的类型保护。

首先我们声明了将要联合的接口。每个接口都有kind属性但有不同的字符串字面量类型。kind属性称做可辨识的特征或标签。其它的属性则特定于各个接口。注意，目前各个接口间是没有联系的。下面我们把它们联合到一起:

```typescript
 interface Rectangle{
     kind:"rectangle";
     width:number;
     height:number;
 }
 interface Circle{
     kind:"circle";
     radius:number;
 }
 function area(s:Shape){
    switch (s.kind) {
        case "Square":
            return s.size * s.size;
           
        case "rectangle":
            return s.height * s.height;
            
        case "circle":
            return Math.PI * s.radius ** 2;
            

    }
 }type Shape = Square | Rectangle | Circle;
 interface Square {
     kind:"Square";
     size:number;
 }
 interface Rectangle{
     kind:"ectangle";
     width:number;
     height:number;
 }
 interface Circle{
     kind:"circle";
     radius:number;
 }
```

## 完整性检查

<font style="color:tomato;">当没有涵盖所有可辨识联合的变化时，我们想让编译器可以通知我们</font>

比如，如果我们添加了 Triangle到 shape，我们同时还需要更新area:

```typescript
type Shape = Square | Rectangle | Circle | Triangle;
function area(s: Shape) :number{
    switch (s.kind) {
        case "Square":
            return s.size * s.size;

        case "rectangle":
            return s.height * s.height;

        case "circle":
            return Math.PI * s.radius ** 2;
        //此处需要跟上新增的Triangle ,不然不起作用，但是TypeScript是不会报错的
        //因为此处要是传入的s是Triangle类型的，那么函数area会返回一个undefined，undefined是any类型的子类型，所以默认不报错
    }
}
```

✍此时我们有2种方法可以实现需求:

<font style="color:skyblue;">启用`--strictNullChecks`并且指定一个返回值类型:</font>

当我们使用了严格空值检查之后，只有**明确指定了**undefined类型的值才允许被设置为空值，否则不允许

此处我们给函数定义了一个数字类型的返回值，—旦我们给函数传入了一个Triangle类型的参数,此时因为case里无法匹配,返回的必然就是`undefined`这个非法值，所以编译器就报错了

<font style="color:skyblue;">第二种方法使用never类型，编译器用它来进行完整性检查:</font>

```typescript
type Shape = Square | Rectangle | Circle | Triangle;
function assertNerver(x: nerver): nerver {
    throw new Error("Unexpected object:" + x)
}
function area(s: Shape) {
    switch (s.kind) {
        case "Square":
            return s.size * s.size;

        case "rectangle":
            return s.height * s.height;

        case "circle":
            return Math.PI * s.radius ** 2;
        default:return assertNerver(s);

    }
}
```

assentNever检s是否为never类型-即为除去所有可能情况后剩卜的类型。如果你际记了某个case，那么 s将具有一个真实的类型并且你会得到一个错误。这种方式需要你定义一个额外的函数，但是在你忘记某个case的时候也更加明显。

## 多态的this类型检查

多态的this类型表示的是某个包含类或接口的子类型。这被称做F-bounded多态性。它能很容易的表现连贯接口间的继承(即所谓的链式操作)

比如。在计算器的例子里，在每个操作之后都返回this类型:

​	或是jQuery函数的返回类型

```typescript
class BasicCalculator{
    public constructor(protected value:number = 0){}
    public currentValue():number{
        return this.value;
    }
    public add(operand:number):this{
        this.value += operand;
        return this;
    }
    public multiply(operand:number):this{
        this.value *= operand;
        return this;
    }
    //...other operations go here...
}
let num = new BasicCalculator(2)
.multiply(9)
.add(1)
.currentValue();
```

由于前页的BasicCalculator这个类使用了 this类型，你可以继承它，新的类可以直接使用之前的方法，不需要做任何的改变。

```typescript
class mathCalculator extends BasicCalculator{
    public constructor(value = 0){
        super(value);
    }
    public sin(){
        this.value = Math.sin(this.value);
        return this;
    }
    //...other operations go here...
}
let numCalc = new mathCalculator(2)
.multiply(520)
.sin()
.add(1)
.currentValue();
```

如果没有this类型, Scientificcalculator就不能够在继承 Basiccalculator的同时还保持接口的连贯性。

## 索引类型(index types)

使用索引类型，编译器就能够检查使用了动态属性名的代码。

例如，一个常见的JavaScript模式是从对象中选取属性的子集。

```typescript
function fn(o, names) {
    return names.map(n => o[n])
}
```

下面代码是如何在Typescript里使用此函数，通过索引类型查询和索引访问操作符:

```typescript
function fn(obj, names) {
    return names.map(n => obj[n])
}
//T代表obj
//keyof:得到T里面所有的属性名,再把它作为一个父类
function fn<T, K extends keyof T>(obj: T, names: K[]): T[K][] {
    return names.map(n => obj[n]);
  //map--新数组的成员都是从obj里面挑选name中所存在的属性,然后组合起来返回
  //对于这样的函数,我们要求返回的一定是obj对象里面的一个子类,name得是obj里面属性名的一个子类
}
interface Person {
    name: string,
    age: number
}
let person: Person = {
    name:"婷宝儿",
    age:18
}
let string:string[] = fn(person,['name']);//ok,string[]
let num:string[] = fn(person,['age']);//err,不能将类型“number”分配给类型string”
```

### 索引类型(index types)之索引类型查询操作符

编译器会检查name是否真的是 Person的一个属性。上面示例还引入了几个新的类型操作符。首先是`keyof T`，**索引类型查询操作符**。对于任何类型T, keyof T的结果为T上已知的公共属性名的联合

```typescript
interface Person {
    name: string,
    age: number
}
let person: Person = {
    name:"婷宝儿",
    age:18
}
let personProps:keyof Person;//"name" | "age"
```

keyof Person是完全可以与‘name' | 'age'互相替换的。不同的是如果你添加了其它的属性到Person，例如address: string，那么keyof Person会自动变为‘name' | 'age' l 'address'。

你可以在像 fn函数这类上下文里使用keyof，因为在使用之前你并不清楚可能出现的属性名。但编译器会检查你是否传入了正确的属性名给fn:

<font style="color:red;">操作符是T[K]，</font>索引访问操作符。

在这里，类型语法反映了表达式语法。这意味着person[ ' name']具有类型Person[ ' name'〕 ,在我们的例子里则为string类型。

然而，就像索引类型查询一样，你可以在普通的上下文里使用T[K]，这正是它的强大所在。你只要确保类型变量K extends keyof T就可以了。

例如下面fn函数的例子:

```typescript
function fn<T, K extends keyof T>(obj: T, names: K): T[K] {
    return obj[names];//obj[names] is type of T[K]
}
```

fn里的obj: T和name: K，意味着obj[name]: T[K]。当你返回T[K]的结果，编译器会实例化键的真实类型，因此 fn的返回值类型会随着你需要的属性改变。

```typescript
function fn<T, K extends keyof T>(obj: T, names: K): T[K] {
    return obj[names];//obj[names] is type of T[K]
}
interface Person {
    name: string,
    age: number
}
let person: Person = {
    name:"婷宝儿",
    age:18
}

let name1: string = fn(person,"name");
let age:number = fn(person,'age');
let unkonwn:number = fn(person,'unKown');//类型“"unKown"”的参数不能赋给类型name"| "age"”的参数。
```

### 索引类型(Index types)之字符串索引签名

<font style="color:red;">keyof和 T[K]与字符串索引签名进行交互</font>

如果你有一个带有字符串索引签名的类型，那么keyof T会是string。并且T[string]为索引签名的类型:

```typescript
interface Map<T>{
    [key:string]:T;
}
let keys:keyof Map<number>;//string
let value:keyof Map<number>['foo'];//number
```

## 映射类型

```typescript
interface objPart{
    name?:string,
    age?:number
}
```

```typescript
interface objReadonly {
    readonly name: string,
    readonly age: number
}
```

—个常见的任务是将一个已知的类型每个属性 或者我们想要一个只读版本都变为可选的:

Typescript提供了从旧类型中创建新类型的一种方式–映射类型。在映射类型里，新类型以相同的形式去转换旧类型里每个属性。

例如，我们可以令每个属性成为readonly类型或可造的。

下面是一些🌰:

```typescript
type Readonly_<T> = {
    readonly [P in keyof T]: T[P];
}
type Part_<T> = {
    [P in keyof T]?: T[P];
}
```

```typescript
type  objPart = Part<Person>;
type objReadonly = Readonly<Person>;
```

饿了么,干饭(本文未完,下篇见)😘