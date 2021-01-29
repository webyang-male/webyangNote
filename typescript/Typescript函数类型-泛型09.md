---
title: Typescript-泛型
date: 2020-11-18 16:36:17
tags: js/typescript
categories: Javascript
---

## 泛型简介

泛型可以理解为是泛指的类型,软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能.

```typescript
function thing(arg:any):any{
    return arg;
}
```

<!-- more -->

创建第一个使用泛型的例子: thing函数。这个函数会返回任何传入它的值

```typescript
function thing(arg: number): number;
function thing(arg: string): string;
function thing(arg: object): object;
//上面代码实现输入什么.返回什么的效果
function thing(arg: any): any {
    return arg;
}
```

使用any类型会导致这个函数可以接收任何类型的arg参数，这样就丢失了一些信息:传入的类型与返回的类型应该是相同的。

但是如果要加上这个判断，代码又会变的格外臃肿

------

### 🌟基本泛型

基于上述，我们需要一种方法使返回值的类型与传入参数的类型是相同的。这里，我们使用了类型变量，它是一种特殊的变量，只用于表示类型而不是值。

```typescript
function thing<T>(arg: T): T {
    return arg;
}
```

我们给thing添加了类型变量T。T帮助我们捕获用户传入的类型（比如: number)，之后我们就可以使用这个类型。之后我们再次使用了T当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。这允许我们跟踪函数里使用的类型的信息。

我们把这个版本的thing函数叫做泛型，因为它可以适用于多个类型。不同于any，它不会丢失信息

### 🌟泛型的常见用法

我们定义了泛型函数后，可以用两种方法使用。

```typescript
let output = thing<string>("mystring");
//这里明确指定了T是string类型,并做为一个参数传给函数,使用了<>括起不是
```

第一种是，传入所有的参数，包含类型参数:

第二种方法更普遍。利用了类型推论--即编译器会根据传入的参数自动地帮助我们确定T的类型:

<font style="color:tomato;">注意我们没必要使用尖括号(<>）来明确地传入类型!</font>

编译器可以查看myString的值，然后把T设置为它的类型。类型推论帮助我们保持代码精简和高可读性。如果编译器不能够自动地推断出类型的话，只能像上面那样明确的传入T的类型，在一些复杂的情况下，这是可能出现的。

## 使用泛型

### 	使用泛型变量

使用泛型创建像thing这样的泛型函数时，编译器要求你在函数体必须正确的使用这个通用的类型。换句话说，你必须把这些参数当做是任意或所有类型。

```typescript
function loggingIdentity<T>(arg:T):T{
    //这里就会直接报错,原因是因为不是每一种数据类型都有length属性的
    console.log(arg.length);
    return arg;
}
```

loggingIdentity的类型:泛型函数loggingIdentity，接收类型参数T和参数arg,arg是个数组项目类型是T的数组，并返回元素类型是T的数组

```typescript
function loggingIdentity<T>(arg:Array<T>):Array<T>{
    console.log(arg.length);
    return arg;
}
function loggingIdentity<array>(arg:Array<string>):Array<string>{
    console.log(arg.length);
    return arg;
}
//不能将类型“number”分配给类型“string"。
loggingIdentity(["1",1]);
```

这可以让我们把泛型变量T当做类型的一部分使用，而不是整个类型，增加了灵活性。

### 使用泛型函数和泛型接口

泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在**最前面**，像函数声明一样:

```typescript
let thing:<U>(arg:U) =>U = function thing<T>(arg:T):T{
    return arg;
}
```

##### 泛型接口的设计与其他的类似

```typescript
interface GenericIdentityFn {
    <T>(arg: T): T;
}
function identity<T>(arg: T):T{
    return arg;
}
let myIdentity: GenericIdentityFn = identity;
```

我们可能想把泛型参数当作整个接口的一个参数。这样我们就能清楚的知道使用的具体是哪个泛型类型（比如:Dictionary<string>而不只是Dictionary)。这样接口里的其它成员也能知道这个参数的类型了。

```typescript
interface GenericIdentityFn<T> {
  //把泛型定义放在接口的后面
  //这样我们在调用这个接口时就可以传入某个具体的类型来细致化接口
    (arg: T): T;
}
function identity<T>(arg: T):T{
    return arg;
}
let myIdentity: GenericIdentityFn<number> = identity;
```

##### 泛型类class

泛型类看上去与泛型接口差不多。泛型类使用（<>）括起泛型类型，跟在类名后面。

```typescript
class GenericNumber<T>{
    zeroValue: T;//该变量的值只能是T类型的
    add: (x: T, y: T)=> T;//该函数传入的参数的类型和返回值的类型都是T类型的
}
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y){ return x +y; };
myGenericNumber.add( 1,2);//没问题
myGenericNumber. add("1",2);//报错，因为该函数只能传入数字类型的参数
```

类有两部分:静态部分和实例部分。泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。(静态类型的一旦定义，就已经成形了)

------

在Typescript使用泛型创建工厂函数时，需要引用构造函数的类类型

```typescript
function create<T>(c: {new():T;}):T {
    //该函数可传入一个︰新的参数，新的函数用过new关键字返回的数据类型得是符合T的类型
    return new c();
}
```

### 泛型约束

前面有个例子,要求实现一个传入什么数据就返回什么数据的函数，一个个的定义每种输入和输出太过麻烦，直接使用泛型，这个有没法细致化的定义输入的要求.

```typescript
function loggingIdentity<T>(arg:T):T{
    //这里就会直接报错,原因是因为不是每一种数据类型都有length属性的
    console.log(arg.length);
    return arg;
}
```

为此，我们定义一个接口来描述约束条件．约束条件就是一个接口，我们使用这个接口和extends关键字来实现约束:

```typescript
interface Lengthwise{
    length: number;//定义了一个接口，改接口表示至少得有一个Length的属性,且值为数字
}

//泛型T拓展自LengthWise接口,使得泛型T多了一个约束条件,那就是必须得是一个用户length属性的数据
function loggingIdentity<T extends Lengthwise>(arg: T):T{
    console.log(arg.length);
    return arg;
}
```

现在这个泛型函数被定义了约束，因此它不再是适用于任意类型.