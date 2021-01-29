---
title: Typescript公共/私有与受保护的修饰符
date: 2020-11-16 21:46:41
tags: js/typescript
categories: Javascript
---

在js的中默认情况下class内的属性和成员我们可以自由的访问，es6允许用户可以手动设置静态方法的方式避免继承

如果你对其它语言中的类比较了解，就会注意到我们在之前的代码里并没有使用public来做修饰;例如，C#， java要求必须明确地使用public指定成员是可见的。

<!-- more -->

#### 🌟<font style="color:tomato;">在TypeScript和es6里，成员都默认public.</font>

```typescript
class Animal {
    public name: string;
    public constructor(theName: string) {
        this.name = theName;
    }
    public move(distanceMeters: number) {
        console.log(`${this.name} moved ${distanceMeters}m.`);

    }
}
let obj = new Animal("熊猫");

console.log(obj.name);//输出 熊猫
console.log(obj.move(520));//输出熊猫移动了520m
```

#### 🌟当成员被标记成private时，它就不能在声明它的类的外部访问。

```typescript
class Animal {
    public name: string;
    public constructor(theName: string) {
        this.name = theName;
    }
    private move(dis:number){
        console.log(`${this.name}移动了${dis}m`);
    }
    private age:18;
    public sayAge(){
        console.log(this.age);
        
    }
}
let obj = new Animal("熊猫");
obj.move(1314);//属性“move”为私有属性，只能在类“Animal”中访间。

Animal.move(1314);//类型“typeof Animal”上不存在属性“move”

```

#### 🌟private私有修饰符

Animal和 Rhino共享了来自Animal里的私有成员定义 private name: string，因此它们是兼容的。

然而Employee却不是这样。当把 Employee赋值给Animal的时候，得到一个错误，说它们的类型不兼容。

```typescript
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}
class Rhino extends Animal {
    constructor() { super("Rhino") };
}

class Employee {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

let animal = new Animal( "Goat");

let rhino = new Rhino();

let employee = new Employee( "Bob");

animal = rhino;
//let animal = new Anima1( "Goat");
animal = employee;//错误: Animal与 EmpLoyee不兼容|
```

尽管Employee里也有一个私有成员name，但它明显不是Animal里面定义的那个。

<font style="color:skyblue;font-weight:900;">不同类里面的同名私有属性，不是同一个属性</font>

------



#### 🌟protected保护修饰符

protected修饰符与private修饰符的行为很相似，但有一点不同, protected成员在派生类中仍然可以访问。

```typescript
class Person {
    protected name: string;
    protected constructor(theName: string) { this.name = theName; }
}

//EmpLoyee能够继承 Person
class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }
    public getElevatorPitch() {
        return `Hello，my name is ${this.name} and I work in ${this.department}. `;

    }
}
let howard = new Employee("Howard", "sales");
let john = new Person("John");//错误: ‘Person’的构造函数是被保护的.
```

#### readonly只读修饰符

你可以使用readonly关键字将属性设置为只读的。只读属性必须在声明时或构造函数里被初始化。

````typescript
class octopus {
  readonly name: string;
  readonly numberOfLegs: number = 8;constructor (theName: string) {
    this.name = theName;
  }
}
let dad = new Octopus( "Man with the 8 strong legs");//只读值被初始化，之后不再允许修改
dad.name = "Man with the 3-piece suit";//错误! name 是只读的.
````

在创造构造函数时，我们有时不仅要限制某个变量后期设置值的时候需要遵守的规则，还要预设一个值的时候，就要用到参数属性，如上述代码中的numberofLegs属性，即设置了值的类型也设置了值的初始值