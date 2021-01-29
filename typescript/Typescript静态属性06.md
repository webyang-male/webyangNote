---
title: Typescript静态属性
date: 2020-11-17 08:55:10
tags: js/typescript
categories: Javascript
---

在ES6部分我们曾学到什么是静态属性:这些属性存在于类本身上面而不是类的实例上，我们只能通过**类.属性**名称的方式来进行调用,而不能在实例上进行调用

<!-- more -->

```typescript
class Grid {
    static origin = { x: 0, y: 0 }
    calculate(point: { x: number, y: number; }) {
        let xDist = (point.x - Grid.origin.x);//这里不能写this.origin
        let yDist = (point.y - Grid.origin.y)
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor(public scale: number) { };
}
let grid1=new Grid(1.0);//1x scale
let grid2=new Grid(8.0);//8x scale

console.log(grid1.calculate({x:10,y:10}));//14.142135623730951
console.log(grid2.calculate({x:12,y:10}));//1.9525624189766635
```

如果代码中标注处写了this.origin的话是会直接报错的,因为构造函数的this默认指的是构造函数生成的实例，而实例是不会继承类的静态方法的.

### 抽象类

抽象类做为其它派生类的基类使用。它们一般不会直接被实例化。不同于接口，抽象类**可以包含成员的实现细节**。

`abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法

------

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。

```typescript
abstract class Animals {
    public name: string;
    public constructor(theName: string) {
        this.name = theName;
    }
    /* public sayName():void{
        console.log(this.name);
    }; */

    /* 
    抽象类Animals里定义了抽象方法,这里的抽象方法是一种格式约束
    那么这种约束就必须要求下方 Person拓展类里面实现
    并且除去 Animals类本来传给下一代Person的public,以及必须实现的abstract之外 
    定义了其他方法不会报错,但是不允许进行使用
    */
    abstract sayName():void;
    abstract sayAge(): void;

}
class Person extends Animals {
    constructor() {
        super("熊猫");
    }
    sayAge(){
        console.log(18);
    }
    /* 
    不加sayName
    类型“Animals”上不存在属性“sayHello”。 
    */
    sayName(){
        console.log(18);
    }
    sayHello(){
        console.log("hello tingbao!");
        
    }
}

let obj1: Animals;
obj1 = new Person();//obj:Animals

obj1.sayAge();
obj1.sayName();

console.log(obj1);//Person { name: '熊猫' }
//obj1.sayHello();//any 类型“Animals”上不存在属性“sayHello”。

```

🌰2

```typescript

abstract class Department {
    constructor(public name: string) { }//相当于定义了一个公共接口name并进行赋值
    printName(): void {
        console.log("Department name: " + this.name);
    }
    abstract printMeeting(): void;//必须在派生类中实现
}
class AccountingDepartment extends Department {
    constructor(){
        super( ' Accounting and Auditing '); //在派生类的构造函数中必须调用 super(
    }
    printMeeting(): void {
        console.log( 'The Accounting Department meets each Monday at 10am. ');
    }
    hello(): void {
        //这个方法不会被继承下去抽象类与接口类似，因为抽象类中没有定义该名称方法,改成其他名字也是一样
        console.log( '你好世界');
    }

}
let department: Department;//允许创建一个对抽象类型的引用
department = new Department();//错误:不能创建一个抽象类的实例
department = new AccountingDepartment();//允许对一个抽象子类进行实例化和赋值
console.dir(department);
department.printName();//继承自父类输出 Department name: Accounting and Auditing
department.printMeeting();//子类自身自带方法输出The Accounting Department meets each Monday at 10am.
department.hello();//错误:方法在声明的抽象类中不存在,
```



<font style="color:red;font-size:20px;font-weight:900;">抽象方法的语法与接口方法相似。</font>

<font style="color:red;font-size:20px;font-weight:900;">两者都是定义方法签名但不包含方法体。</font>然而，抽象方法必须包含abstract关键字