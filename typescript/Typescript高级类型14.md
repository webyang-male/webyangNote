---
title: Typescript高级类型03
date: 2020-12-07 15:59:53
tags: js/typescript
categories: Javascript
description: 我保证这是"最后"一篇ts高级类型文章了😎
---

注意Readonly<T>和Partial<T>用处不小，因此它们与Pick和Record一同被包含进了TypeScript的标准库里:

```typescript
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
}
type Record<K extends string, T> = {
    [P in K]: T;
}
```

Readonly,Partial和Pick是同态的，但Record不是。因为 Record并不需要输入类型来拷贝属性，所以它不属于同态:

```typescript
type ThreestringProps = Record<' prop1' | 'prop2' | 'prop3 ', string>
```

<font style="color:red;">非同态类型本质上会创建新的属性，因此它们不会从它处拷贝属性修饰符</font>

```typescript
type Keys = 'option1' | 'option2';
type Flags = {[K in Keys]:boolean};
```

最简单的映射类型和它的组成部分:

它的语法与索引签名的语法类型，内部使用了for .. in。具有三个部分:

1．类型变量K，它会依次绑定到每个属性。

2．字符串字面量联合的Keys，它包含了要迭代的属性名的集合。

3．属性的结果类型。

在这个简单的例子里, Keys是硬编码的的属性名列表并且属性类型永远是boolean，因此这个映射类型等同于:

```typescript
type Flags = {
    option1:boolean;
    option2:boolean;
}
```

### 映射类型

在真正的应用里，可能不同于上面的 Readonly或 Part。它们会基于一些已存在的类型，且按照一定的方式转换字段。这就是keyof和索引访问类型要做的事情

```typescript
interface Person {
    name: string;
    age: number;
}
let person: Person = {
    name: '婷宝儿',
    age: 18
}
type NullPerson = { [P in keyof Person]: Person[P] | null };
//新的类型可以是包含了person里的每一个规则的同时还可以设置null
type PartPerson = { [P in keyof Person]?: Person[P]};
//新的类型是包含了person里的每一个规则的同时,每个都是可选属性

```

通用版本:

```typescript
type NullPerson<T> = { [P in keyof T]: T[P] | null };
type PartPerson<T> = { [P in keyof T]?: T[P]};
```

### 由映射类型进行推断

现在咱们了解了如何包装一个类型的属性，那么接下来就是如何拆包。

```typescript
type Proxy<T> = {
    get():T;
    get(value:T):void;
}
type Proxify<T> = {
    [P in keyof T]:Proxy<T[P]>;
}
function proxify<T>(o:T):Proxify<T>{
    return;
}
```

````typescript
function Unproxify<T>(t: Proxify<T>): T {
    let result = {} as T;
    for (const k in t) {
        result[k] = t[k].get()
    }
    return result;
}
let originalProps = Unproxify(ProxyProps);
````

注意这个拆包推断只适用于同态的映射类型。如果映射类型不是同态的，那么需要给拆包函数一个明确的类型参数。

**TypeScript 2.8在lib.d.ts里增加了一些预定义的有条件类型:**

1. Exclude<T, U> --从T中剔除可以赋值给u的类型
2. Extract<T,U> --提取T中可以赋值给u的类型
3. NonNullable<T> -- 从T中剔除null和undefined
4. ReturnType<T> --获取函数返回值类型InstanceType<T> --获取构造函数类型的实例类型

```typescript
type T00 = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">;//"b"|"d"
type T01 = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f">;//"a"|"c"

type T02 = Exclude<string | number | (() => void), Function>;//string | number
type T03 = Extract<string | number | (() => void), Function>;//()=>void

type T04 = NonNullable<string | number | undefined>;//string | number
type T05 = NonNullable<(() => string) | string[] | null | undefined>;//(() => string) | string[]

function f1(s:string){
    return {a:1,b:s};
}
```