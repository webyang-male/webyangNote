---
title: Typescript高级类型01
date: 2020-12-02 22:29:15
description: 后面还you高级类型博客哦~🔥
tags: js/typescript
categories: Javascript
---

## 交叉类型

<font  style="color:red;">交叉类型是将多个类型合并为一个类型。</font>

<font  style="color:red;">这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。</font>
<!-- more -->

例如，Person & Serializable & Loggable同时是Person和Serializable 和Loggable。就是说这个类型的对象同时拥有了这三种类型的成员

```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};//类型断言,并集
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
      //首先排除存在的id值
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLog implements Loggable {
    log() {

    }
}
var zzy = extend(new Person("zzy"), new ConsoleLog());
var tingbao = zzy.name;
zzy.log();
```

编译:

```javascript
function extend(first, second) {
    var result = {};
    for (var id in first) {
        result[id] = first[id];
    }
    for (var id in second) {
        if (!result.hasOwnProperty(id)) {
            result[id] = second[id];
        }
    }
    return result;
}
var Person = /** @class */ (function () {
    function Person(name) {
        this.name = name;
    }
    return Person;
}());
var ConsoleLog = /** @class */ (function () {
    function ConsoleLog() {
    }
    ConsoleLog.prototype.log = function () {
    };
    return ConsoleLog;
}());
var zzy = extend(new Person("zzy"), new ConsoleLog());
var tingbao = zzy.name;
zzy.log();

```

## 联合类型

联合类型与交叉类型很有关联，但是使用上却完全不同。

偶尔我们会遇到这种情况，一个代码库希望传入number或string类型的参数

```typescript
function fn(value: string, padding: any) {
    if (typeof padding === "number") {
        return Array(padding + 1).join("") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Not string or number,got '${padding}'.`);
}
fn("Hello world", 4);
```

fn存在一个问题, padding参数的类型指定成了 any。这就是说我们可以传入一个既不是number也不是string类型的参数，但是TypeScript却不报错。

------

联合类型表示一个值可以是几种类型之一。我们用竖线（ | ）分隔每个类型，所以number / string | boolean表示一个值可以是number, string，或boolean

```typescript
function fn(value: string, padding: string|number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join("") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Not string or number,got '${padding}'.`);
}
fn("Hello world", true);//err,因为true不是字符串或者数字
```

如果一个值是联合类型，我们只能访问此联合类型的所有类型里<font  style="color:hotpink;">共有的成员。</font>

```typescript
interface Bird{
    fly();
    layEggs();
}
interface Fish{
    swim();
    layEggs();
}
function getAnimal():Fish | Bird{
    return{
        layEggs(){},
        swim(){},
        fly(){}
    };
}
let pet = getAnimal();
pet.layEggs();//正确
pet.swim();//err,类型"Bird | Fish"上不存在属性"swim"
```

如果—个值的类型是A | B，我们能够确定的是它包含了A和B中共有的成员。这个例子里,Bird具有一个fly成员。但是Fish里面没有f1y。如果变量在运行时是Fish类型，那么调用pet.fly()就出错了。

## 类型保护

##### 	类型保护与区分类型

联合类型适合于那些值可以为不同类型的情况。

但是如果我们就想要来判断—下，最后的pet对象是否拥有swim和fly属性的时候我们又该咋办?

传统的方式就是在if检查成员是否存在，但是Typescript的类型检查会直接报错，根本不给检测的机会.

```typescript
interface Bird {
    fly();
    layEggs();
}
interface Fish {
    swim();
    layEggs();
}
function getAnimal(): Fish | Bird {
    return {
        layEggs() { },
        swim() { },
        fly() { }
    };
}
let pet = getAnimal();
//每一个成员访问都会报错
if (pet.swim) {
    pet.swim();
} else if (pet.fly) {
    pet.fly();
}
```

此时我们就可以使用类型断言的方式来强行指定类型来绕过TypeScript的排查机制

```typescript
let pet = getSmallPet();

if((<fish>pet).swim){
    (<fish>pet).swim();
}else{
    (<Bird>pet).fly();
}
```

上方的代码中，为了确保能够正常判断，我们用了很多次类型断言。这种方法太过麻烦了

假若我们—旦检查过类型，就能在之后的每个分支里清楚地知道pet的类型的话就好了。

Typescript里的类型保护机制让它成为了现实。类型保护就是一些表达式，它们会在运行时检查以确保在某个作用域里的类型。要定义一个类型保护，我们只要简单地定义一个函数，它的返回值是一个类型谓词;

```typescript
interface Bird {
    fly();
    layEggs();
}
interface Fish {
    swim();
    layEggs();
}
function getAnimal(): Fish | Bird {
    return {
        layEggs() { },
        swim() { },
        fly() { }
    };
}
let pet = getAnimal();

function isFish(pet:Fish | Bird):pet is Fish{
    return (<Fish>pet).swim ! == undefined;
}

```

在上面例子里, pet is Fish就是类型谓词。谓词为parameterName is Type这种形式,parameterName必须是来自于当前函数签名里的一个参数名。

🐙每当使用—些变量调用isFish时，Typescript会将变量缩减为那个具体的类型，只要这个类型与变量的原始类型是兼容的。

```typescript
interface Bird {
    fly();
    layEggs();
}
interface Fish {
    swim();
    layEggs();
}
function getAnimal(): Fish | Bird {
    return {
        layEggs() { },
        swim() { },
        fly() { }
    };
}
let pet = getAnimal();

function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim! == undefined;
}

//swim和fly调用都没有问题
if (isFish(pet)) {
    pet.swim();
} else {
    pet.fly();
}
```

------

##### typeof类型保护

如下，必须要定义—个函数来判断类型是否是原始类型，这太tk了。

```javascript
function isNumber(x: any): x is number {
    return typeof x === "number";
}
function isString(x: any): x is string {
    return typeof x === "string";
}
function paddingLeft(value: string, padding: string | number) {
    if (isNumber(padding)) {
        return Array(padding + 1).join("") + value;
    }
    if (isString(padding)) {
        return padding + value;
    }
    throw new console.error(`Not string or number,got '${padding}'.`);
    
}
```

幸运的是，现在我们不必将typeof x=== "number"抽象成一个函数，因为TypeScript可以将它识别为一个类型保护

也就是说我们可以直接在代码里检查类型了

```typescript
function paddingLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join("") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new console.error(`Not string or number,got '${padding}'.`);
    
}
```

```javascript
这些typeof类型保护只有两种形式能被识别:typeof v === "typename"和 typeof v !=="typename"，"typename"必须是“number"，"string","boolean"或"symbol"。
但是Typescript并不会阻止你与其它字符串比较，语言不会把那些表达式识别为类型保护。
```

##### instanceof类型保护

instanceof类型保护是通过构造函数来细化类型的一种方式

instanceof的右侧要求是─个构造函数，Typescript将细化为:

A．此构造函数的prototype属性的类型，如果它的类型不为any的话

B．构造签名所返回的类型的联合

以此顺序.

```typescript
interface Padder  {
    getPaddingString(): string
}
class SpaceRepeatingPadder implements Padder{
    constructor(private numSpaces : number) { }
    getPaddingString(){
        return Array (this. numSpaces + 1).join(" ");
    }
}
class StringPadder implements Padder {
    constructor(private value: string){}
    getPaddingString(){
        return this.value;
    }
}
function getRandomPadder(){
    return Math.random() < 0.5 ?
    new SpaceRepeatingPadder(4):
    new StringPadder(" ");
}
//类型为SpaceRepeatingPadder | stringPadder
let padder: Padder = getRandomPadder();
if (padder instanceof SpaceRepeatingPadder) {
    padder;//类型细化为'SpaceRepeatingPadder'
}
if (padder instanceof StringPadder) {
    padder;//类型细化为'StringPadder'
}
```

##### 为null的类型

`--strictNullChecks(严格空值检查)`标记可以解决此错误:当你声明一个变量时，它不会自动地包含nul1或undefined(即, undefined和null再也不是任意类型的子类型了)。

你可以使用联合类型明确的包含它们:

```typescript
let zzy = "hansome";
zzy = null;//错误,"null"不能赋值给'string'
let obj:string | null = "tingbao";
obj = null;//可以
obj = undefined;//error, 'undefined ·不能赋值给'string / null'
```

![ts5f63dfe73aa05f61.png](https://b2.kuibu.net/file/imgdisk/2020/12/02/ts5f63dfe73aa05f61.png)

注意，按照JavaScript的语义，TypeScript会把null和 undefined区别对待。string | null, string | undefined和string | undefined | null是不同的类型。

##### 为null的类型之可选参数

使用了--strictNullChecks，可选参数会被自动地加上| undefined :

```typescript
function test(x: number, y?: string){
    //y ? : number 在strictNulLChecks模式下变成了y? :number | undefined
}
test(1);
//Argument of type 'null' is not assignable to parameter of type 'string | undefined'.
test(1,null);
test(1, undefined);
```

可选属性也会有同样的处理:

![-2020-12-02-213353-169dfb9a812976eea.png](https://cdn.webchain.site/file/imgdisk-2/2020/12/02/-2020-12-02-213353-169dfb9a812976eea.png)

##### 为null的类型之类型保护和类型断言

由于可以为null的类型是通过联合类型实现，那么你需要使用类型保护来去除null

```typescript
//这里很明显地除去了null
function f(sn:string | null):string{
    if (sn == null) {
        return "default";
    }else{
        return sn;
    }
} 
```

也可以使用短路运算符

```typescript
function f(sn:string | null):string{
        return  sn || "default";
}
```

如果编译器不能够去除null或undefined，你可以使用类型断言手动去除。语法是添加`!后缀`: identifier!从 identifier的类型里去除了null和undefined:

```typescript
function amazing(name) {
    function postAmaz(x) {
        return name.charAt(0) + '. the' + x; //正确
    }
    name = name || "婷宝儿";
    return postAmaz("lovely girl");
}
```

本例使用了嵌套函数，因为编译器无法去除嵌套函数的null(除非是立即调用的函数表达式)。因为它无法跟踪所有对嵌套函数的调用，尤其是你将内层函数做为外层函数的返回值。如果无法知道函数在哪里被调用，就无法知道调用时name的类型。

##### 类型别名

> 语法:type <font style="color:gold;"> 新类型名称</font> =  <font style="color:gold;">原始类型名称</font>;

类型别名会给一个类型起个新名字。类型别名有时和接口很像，但是可以作用于原始值，联合类型，元组以及其它任何你需要手写的类型。

```typescript
type Container<T> = {value:T};
```

同接口—样，类型别名也可以是泛型–我们可以添加类型参数并且在别名声明的右侧传入.

```typescript
type Tree<T> = {
    value:T;
    left:Tree<T>;
    right:Tree<T>;
}
```

然而，类型别名不能出现在声明右侧的任何地方:

```typescript
type zzy = Array<zzy>;
```

##### 类型别名

```typescript
type LinkedList<T> = T & {next :LinkedList<T>};
interface Person{
    name:string;
}
var people:LinkedList<People>;
var s = people.name;
var s = people.next.name;
var s = people.next.next.name;
```

与交叉类型—起使用，我们可以创建出一些十分稀奇古怪的类型。