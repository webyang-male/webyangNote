---
title: Typescript类型推论与兼容
date: 2020-11-27 17:50:49
tags: js/typescript
categories: Javascript
---

## 类型推论

### 基础类型推论

<font  style="color:deepskyblue;">Typescript里，在有些没有明确指出类型的地方，类型推论会帮助提供类型.</font>

<!-- more -->

```typescript
let tingBaoAge = 18;
/* 此处虽然没有直接声明x变量的值的类型，但是因为tingBaoAge被赋值为数字18
那么x变量的数据类型就会被自动限制为数字
 */
tingBaoAge = "18";//err,不能将类型“string”分配给类型“number”。
```

这种推断发生在<font  style="color:red;">初始化变量和成员</font>，<font  style="color:red;">设置默认参数值</font>和<font  style="color:red;">决定函数返回值</font>时。

### 最佳通用类型

当需要从几个表达式中推断类型时候，会使用这些表达式的类型来推断出一个最合适的通用类型

```typescript
let tingBaoAge = [18,99,520,null,undefined];

tingBaoAge.push("18");//err,不允许往该数组内添加字符串
```

为了推断x的类型，我们必须考虑所有元素的类型。这里有3种选择: **number,undefined和null**。计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型。

✍由于最终的通用类型取自候选类型，<font  style="color:red;">有些时候候选类型共享相同的通用类型，但是却没有一个类型能做为所有候选类型的类型</font>。例如:

```typescript
let zoo = [new Snake(),new Elephant(),new Monkey()];
```

这里，我们想让zoo被推断为Animal[]类型，但是这个数组里没有对象是Animal类型的，因此不能推断出这个结果。为了更正，当候选类型不能使用的时候我们需要明确的指出类型:

```typescript
let zoo:Animal = [new Snake(),new Elephant(),new Monkey()];
```

如果没有找到最佳通用类型的话，类型推断的结果为联合数组类型，(Rhino |Elephant | Snake)[ ]。

### 上下文类型推论(上下文归类)

Typescript类型推论也可能按照相反的方向进行。这被叫做“按上下文归类”。按上下文归类会发生在表达式的类型与所处的位置相关时

```js
window.onmousedown = function(mouseEvent){
    console.log(mouseEvent.button);
}
```

Typescript类型检查器使用window. onmousedown函数的类型来推断右边函数表达式的类型。因此，就能推断出mouseEvent参数的类型了

如果上下文类型表达式包含了明确的类型信息，上下文的类型被忽略:

```typescript
window.onmousedown = function(mouseEvent:any){
    console.log(mouseEvent.button);
}
```

✏️上下文归类会在很多情况下使用到:

1. 通常包含函数的参数

2. 赋值表达式的右边

3. 类型断言

4. 对象成员和数组字面量和返回值语句

5. 上下文类型也会作为最佳通用类型的候选类型

   

   ```typescript
   function createzoo(): Animal[] {
       return [new Monkey(), new Elephant(), new Snake()];
   }
   ```

这个例子里，最佳通用类型有4个候选者: Animal，Monkey，Elephant和Snake。当然，<font  style="color:skyblue;">Animal</font>会被做为最佳通用类型。





## 类型兼容

### 类型兼容模型

目前在所有的编程语言中，对于类型兼容的实现除了结构类型外，还有一个叫做名义类型，结构类型和名义类型之间的区别如下:

1. 在基于名义类型的类型系统中，数据类型的兼容性或等价性是通过明确的声明和/或类型的名称来决定的
2. 在基于结构性的类型系统中，数据类型的兼容性或等价性是基于类型的组成结构，且不要求明确地声明。

<font  style="color:lightskyblue;">Typescript里的类型兼容性是基于结构子类型的，使用其成员来描述类型的方式</font>

```typescript
interface Name {
    name: string;
}
class Person {
    name: string;
}

let x: Name;

x = new Person();//正确,这个符合格式,因为Preson的实例里面有一个名为name的属性,这完全符合接口的要求
```

### 结构化类型系统

TypeScript的结构性子类型是根据Javascript代码的典型写法来设计的。因为JavaScript里广泛地使用匿名对象，例如函数表达式和对象字面量，所以使用结构类型系统来描述这些类型比使用名义类型系统更好。

<font  style="color:red;font-size:20px">Typescript结构化类型系统的基本规则是，如果m要兼容n，那么n至少具有与m相同的属性</font>

```typescript
interface Name {
    name: string;
}

let x: Name;
let y = {name:"tingbao",boyfriend:"zzy"};
x = y;
```

```typescript
interface Name {
    name: string;
}

let x: Name;
let y = {name:"tingbao",boyfriend:"zzy"};
x = y;
console.log(x);//{ name: 'tingbao', boyfriend: 'zzy' }
function love(x:Name){
    console.log('I love u!'+ x.name);
    
}
love(y);//正确
```

这里要检查y是否能赋值给x，编译器检查x中的每个属性，看是否能在y中也找到对应属性。在这个例子中，y必须包含名字是name的string类型成员。y满足条件，因此赋值正确。

🔺 y有个额外的boyfriend属性，但这不会引发错误。只有目标类型(这里是Named)的成员会被一 一检查是否兼容。

​	这个比较过程是递归进行的，检查每个成员及子成员。

````typescript
interface Name {
    name: string;
    sex:string;
    age:number
}

let x: Name;
let y = {name:"tingbao",sex:"女"};
x = y;//err,类型"{name: string; sex: string; }”中缺少属性“age"，但类型“Name”中需要该属性。
//y属性可以多,但是不能少
````

#### 函数的类型比较

```typescript
let x =(a: number) => 0;
let y =(b: number, s: string) => 0;
y = x; // OK
x = y; // Error不能将类型"(b: number， s: string) =>number”分配给类型"(a: number) => number”
```

要查看x是否能赋值给y，首先看它们的<font  style="color:gold;">参数列表</font>。x的每个参数<font  style="color:skyblue;">必须能</font>在y里找到对应类型的参数。

🔺注意:参数的名字相同与否无所谓，只看它们的类型。这里，x的每个参数在y中都能找到对应的参数，所以允许赋值。

但是反过来就不行了,第二个赋值错误，因为y有个必需的第二个参数，但是x并没有，所以不允许赋值。

这里面有一个点需要注意，在第一个赋值例子代码中,之所以x可以赋值给y的原因就是JavaScript的函数允许实参少于形参

**比如:**

Array.forEach给回调函数传3个参数:数组元素，索引和整个数组。尽管如此，传入一个只使用第一个参数的回调函数也是很有用的:

<font  style="color:tomato;">	**简而言之，TypeScript的函数类型比较里面,参数可以少,但不能多**</font>

------

不过<font  style="color:yellowgreen;">函数的返回值</font>进行比较的时候，就有些不同了，<font  style="color:tomato;">可以多，但不可以少</font>

```typescript
let x=() => ({name: 'Alice'});
let y = ()=> (iname: 'Alice', location: 'seattle '});
x = y;//right 变量x是源,变量y是目标
y = x;//Error, because x() Lacks a Location property
```

类型系统强制源函数的返回值类型必须是目标函数返回值类型的子类型。

#### 剩余参数

比较函数兼容性的时候，可选参数与必须参数是可互换的。源类型上有额外的可选参数不是错误，目标类型的可选参数在源类型里没有对应的参数也不是错误。

```typescript
function invokeLater(args: any[],callback: (...args: any[]) => void){
    /* ... Invoke callback with 'args' ...*/
}

invokeLater([1,2],(x,y) => console.log(x + ', ' + y));//正确

invokeLater([1,2],(x?, y?) => console.log(x + ', '+ y));
//正确，因为对剩余这个概念来说，有的剩和没得剩都是一样的
```

当一个函数有剩余参数时，它被当做无限个可选参数。这对于类型系统来说是不稳定的，但从运行时的角度来看，可选参数一般来说是不强制的，因为对于大多数函数来说相当于传递了一些undefined。

#### 枚举的类型比较

<font  style="color:red;font-size:18px">枚举类型与数字类型兼容，并且数字类型与枚举类型兼容。</font>不同枚举类型之间是不兼容的。

```typescript
enum Status { Ready, Waiting };
enum Color { Red, Blue, Green };

let s = Status.Ready;
s = 99;//正确
s="520"//不能将类型“520""分配给类型“Status”。
s=Color.Blue;//不能将类型“color.Blue”分配给类型“Status”
```

#### 类的类型比较

类与对象字面量和接口差不多，但有一点不同:<font  style="color:gold;font-size:18px">类有静态部分和实例部分的类型</font>

比较两个类类型的对象时，只有实例的成员会被比较。静态成员和构造函数不在比较的范围内

```typescript
import { isConstructorDeclaration } from "typescript";

class Animal {
    foot: number;
    static sayMyName() {
        console.log("panda");

    };
    constructor(name: string, numFoot) { };
}
class Size {
    foot: number;
    constructor(numFoot: number) { };
}
let a: Animal;
let b: Size;

a = b;//正确
b = a;//正确
```

类的私有成员和受保护成员会影响兼容性。

当检查类实例的兼容时，<font  style="color:red;">如果目标类型包含一个私有成员，那么源类型必须包含来自同一个类的这个私有成员</font>。同样地，这条规则也适用于包含受保护成员实例的类型检查。这允许子类赋值给父类，但是不能赋值给其它有同样类型的类。

#### 泛型的类型兼容性

因为Typescript是结构性的类型系统，类型参数只影响使用其做为类型一部分的结果类型。

```typescript
interface Empty<T>{
//空泛型
}
let x:Empty<number>;
let y:Empty<string>;

x = y;
```

上面代码里，x和y是兼容的，因为它们的结构使用类型参数时并没有什么不同。

```typescript
interface notEmpty<T> {
    data: T;
}
let x: notEmpty<number>;
let y: notEmpty<string>;
/* 不能将类型“notEmpty<string>”分配给类型"notEmpty<number>”。
不能将类型“string”分配给类型“number”。 */
x = y;//err
```

在这里，泛型类型在使用时就好比不是一个泛型类型。

------

对于没指定泛型类型的泛型参数时，会把所有泛型参数当成<font  style="color:skyblue;">any</font>比较。然后用结果类型进行比较。

```typescript
let obj = function<T>(x:T):T{

}

let someThing = function<U>(y:U):U{
    
}

obj = someThing;
```

## 子类型与赋值的理论辨析

我们使用了“兼容性”，它在语言规范里没有定义。在Typescript里，有两种兼容性:**子类型和赋值**。

它们的不同点在于，赋值扩展了子类型兼容性，增加了一些规则，允许和any来回赋值，以及enum和对应数字值之间的来回赋值。

语言里的不同地方分别使用了它们之中的机制。

<font  style="color:red;">实际上，类型兼容性是由赋值兼容性来控制的，即使在implements和extends语句也不例外。</font>

