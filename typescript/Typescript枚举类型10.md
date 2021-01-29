---
title: Typescript枚举类型
date: 2020-11-25 09:11:02
tags: js/typescript
categories: Javascript
---

### 🌟数字枚举

使用枚举我们可以定义一些带名字的常量。使用枚举可以清晰地表达意图或创建一组有区别的用例。Typescript支持数字的和基于字符串的枚举。创造枚举类型的值的关键字是enum.

```typescript
enum Direction{
    Up = 1,
    Down,
    Left,
  	Right
}
```

如上，我们定义了一个数字枚举，up使用初始化为1(这也被成为初始化器)。其余的成员会从1开始自动增长。换句话说，Direction.Up的值为1，Down为2，Left为3，Right为4。

```typescript
enum Direction{
    Up,
    Down,
    Left,
  	Right
}
```

我们还可以完全不使用初始化器:

现在，up的值为0,Down的值为1等等。当我们不在乎成员的值的时候，这种自增长的行为是很有用处的，但是要注意每个枚举成员的值都是不同的。

------

枚举类型response的最终值本质来讲,枚举类型是一个名值对,和值名对的结合体，可以通过值来访问键名,也可以通过键名访问值

```typescript
enum obj{
    a,
    b,
    c
}
--------------------------------
//js编译后
var obj;
(function (obj) {
    obj[obj["a"] = 0] = "a";
    obj[obj["b"] = 1] = "b";
    obj[obj["c"] = 2] = "c";
})(obj || (obj = {}));
console.log(obj);//{ '0': 'a', '1': 'b', '2': 'c', a: 0, b: 1, c: 2 }
```

------

```typescript
function getValue(){
    return 99;
}

enum obj{
    a,//0
    b = getValue(),
    c//err 枚举成员必须具有初始化表达式

    //c = getValue() 正确
    // getValue函数放在前面,例如变量a也同样错误
}

function getValue1(){
    return 99;
}

enum obj1{
    e,//0
    f = getValue1(),
    g = 9,
    d
}
```

不带初始化器的枚举要么放在倒数第一的位置，要么被放在使用了数字常量或其它常量初始化了的枚举后面。

------

### 🌟其他枚举

#### 字符串枚举

字符串枚举的概念很简单，但是有细微的运行时的差别。在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。

````typescript
enum obj1 {
    e = "婷宝儿",
    f = "zzy",
    g = "1314",
    d = "99"

}
````

ts编译后:

```javascript
var obj1;
(function (obj1) {
    obj1["e"] = "\u5A77\u5B9D\u513F";
    obj1["f"] = "zzy";
    obj1["g"] = "1314";
    obj1["d"] = "99";
})(obj1 || (obj1 = {}));

```

由于字符串枚举没有自增长的行为，字符串枚举可以很好的序列化。换句话说，如果你正在调试并且必须要读一个数字枚举的运行时的值，这个值通常是很难读的–它并不能表达有用的信息（尽管反向映射会有所帮助)，字符串枚举允许你提供一个运行时有意义的并且可读的值，独立于枚举成员的名字。

🔹如果尝试将值换成**对象**写法:

```typescript
enum obj1 {
    e = {name:"婷宝儿",age:"18"},//err 含字符串值成员的枚举中不允许使用计算值
    f = "zzy",
    g = "1314",
    d = "99"

}
enum obj2 {
    e = {name:"婷宝儿"},//err 含字符串值成员的枚举中不允许使用计算值
}
```

🔹如果尝试将其中一个值换成**数值**写法:

```typescript
enum obj1 {
    e = "tingbao",
    f = "zzy",
    g = 1314,
    d = "99"
}
console.log(obj1);
```

ts编译后:

```javascript
//编译通过
var obj1;
(function (obj1) {
    obj1["e"] = "tingbao";
    obj1["f"] = "zzy";
    obj1[obj1["g"] = 1314] = "g";
    obj1["d"] = "99";
})(obj1 || (obj1 = {}));
console.log(obj1);//{ '1314': 'g', e: 'tingbao', f: 'zzy', g: 1314, d: '99' }
```

🔹再尝试将第一个值不写

```typescript
enum obj1 {
    e,
    f = "zzy",
    g = 1314,
    d = "99"
}
console.log(obj1);
```

ts编译后:

```javascript
var obj1;
(function (obj1) {
    obj1[obj1["e"] = 0] = "e";//注意这一句
    obj1["f"] = "zzy";
    obj1[obj1["g"] = 1314] = "g";
    obj1["d"] = "99";
})(obj1 || (obj1 = {}));
console.log(obj1);//{ '0': 'e', '1314': 'g', e: 0, f: 'zzy', g: 1314, d: '99' }
```

------

#### 异构(Heterogeneou)枚举

枚举可以混合字符串和数字成员,但是一般不太建议

```typescript
enum value {
    f = "zzy",
    g = 1314,
}
```

------

#### 计算成员

每个枚举成员都带有一个值，它可以是常量或计算出来的。

当满足如下条件时，枚举成员被当作是常量:

它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值0:

```typescript
enum value {
   x 
}
console.log(value);//{ '0': 'x', x: 0 }
```

它不带有初始化器且它之前的枚举成员是一个数字常量。这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。

```typescript
enum tingbao { x, y, z };
enum zzy{
    o =1,p,q
}
console.log(tingbao);
console.log(zzy);
// { '0': 'x', '1': 'y', '2': 'z', x: 0, y: 1, z: 2 }
// { '1': 'o', '2': 'p', '3': 'q', o: 1, p: 2, q: 3 }
```

**枚举成员使用常量枚举表达式初始化。常数枚举表达式是Typescript表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，它就是一个常量枚举表达式:**

1. 一个枚举表达式字面量（主要是字符串字面量或数字字面量)
2. 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的)

```typescript
enum tingbao { x, y, z };
enum zzy{
    o =tingbao.x,p,q
}
```

 3.带括号的常量枚举表达式(<font style="color:red">表达式返回值是常量,但是这个初始化没法用函数的返回值</font>)

 4.一元运算符+, -，~其中之一应用在了常量枚举表达式

 5.常量枚举表达式做为二元运算符+，-,*，/，%，<<，>>，>>>，&，|，^的操作对象。若常数枚举表达式求值后为NaN或Infinity，则会在编译阶段报错。

------

#### 联合枚举

存在一种特殊的非计算的常量枚举成员的子集:字面量枚举成员。字面量枚举成员是指<font style="color:skyblue">不带有初始值</font>的常量枚举成员，或者是值被<font style="color:gold">初始化</font>为

1. 任何字符串字面量（例如:"foo"，"shapeLength","value")
2. 任何数字字面量（例如: 1, 10日)
3. 应用了一元 -符号的数字字面量（例如:-1，-99)

```typescript
enum Shape{
    //不带初始值的常量枚举,默认从0开始
    Circle,//ts默认设置为0
    Square,//ts默认设置为1
}
interface Circle{
    kind:Shape.Circle;
    radius:number;
}

interface Square{
    kind:Shape.Square;
    shapeLength:number;
}

let c:Circle = {
    //err,所需类型来自属性“kind",在此处的“circle”类型上声明该属性化为
    kind:Shape.Square,//err不能将类型“shape.Square”分配给类型“shape.Circle”。所需类型来自属性“kind",在此处的“circle”类型上声明该属性
    radius:99
}
```

当所有枚举成员都拥有字面量枚举值时，它就带有了一种特殊的语义。

*首先，枚举成员成为了类型*

例如，我们可以说某些成员只能是枚举成员的值

另一个变化是枚举类型本身变成了**每个枚举成员的联合**。通过联合枚举，类型系统能够利用这样一个事实,它可以知道枚举里的值的集合。因此，TypeScript能够捕获在比较值的时候犯的愚蠢的错误。例如:

```typescript
enum tingbao {
    name,
    sex
}
function f(x: tingbao) {
    //此条件将始终返回“true"，因为类型“tingbao.name”和“tingbao.sex”没有重叠。
    if (x !== tingbao.name || x !== tingbao.sex) {
        console.log("条件通过");
    }else{
        console.error("条件不通过")
    }
}
```

这个例子里，我们先检查x是否不是tingbao.name。如果通过了这个检查，然后||会发生短路效果，if语句体里的内容会被执行。然而，这个检查没有通过，那么x则只能为tingbao.name，因此没理由再去检查它是否为tingbao.sex.

------

#### 运行时的枚举

```typescript
enum E {
   X,Y,Z
}
function fn(obj:{X:number}){
    return obj.X;
}
fn(E);//这个是完全没有问题的，E里面就是有一个值为数字的X属性
```

------

#### 反向映射

除了创建一个以属性名做为对象成员的对象之外，数字枚举成员还具有了反向映射，从枚举值到枚举名字。

即可以通过名称得到值，也可以通过值得到名称.

```typescript
enum E {
   z
 }
 let a= E.z;
 let nameOfa = E[a];//"z"
```

编译后:

```js
var E;
(function (E) {
    E[E["z"] = 0] = "z";
})(E || (E = {}));
var a = E.z;
var nameOfa = E[a];
console.log(nameOfa);//z
```

生成的代码中，枚举类型被编译成一个对象，它包含了正向映射（ name -> value)和反向映射( value -> name)。引用枚举成员总会生成为对属性访问并且永远也不会内联代码。

⚠️要注意的是不会为字符串枚举成员生成反向映射。

------

#### const枚举/常量枚举

大多数情况下，枚举是十分有效的方案。然而在某些情况下需求很严格。为了避免在额外生成的代码上的开销和额外的非直接的对枚举成员的访问，我们可以使用const枚举。

<font style="color:hotpink">常量枚举通过在枚举上使用const修饰符来定义</font>

```typescript
const enum Enum {
    a = 1,
    b = a * 99
}
```

ts编译后

```js
//js文件内容为空,无法编译
```

常量枚举，编译后会被删的渣都不剩

```typescript
const enum Position {
   up,
   down,
   left,
   right
}
let positon = [Position.up,Position.down,Position.left,Position.right];
```

ts编译后

```javascript
var positon = [0 /* up */, 1 /* down */, 2 /* left */, 3 /* right */];
```

常量枚举只能使用常量枚举表达式(不能用取值函数)，并且不同于常规的枚举，它们在编译阶段会被删除。常量枚举成员在使用的地方会被内联进来。之所以可以这么做是因为，常量枚举不允许包含计算成员。

------

#### 外部枚举

外部枚举用来描述已经存在的枚举类型的形状

````typescript
declare enum Enum{
    a =1,
    b,
    c = 2
}
````

外部枚举和非外部枚举之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常数成员。对于非常数的外部枚举而言,没有初始化方法时被当做需要经过计算的。