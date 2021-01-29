---
title: ts模块系统
date: 2020-12-09 15:45:42
tags: js/typescript
categories: Javascript
---

## 模块的专业术语变更

<font style="color:red;font-size:18px">“内部模块”现在称做“命名空间”;“外部模块”现在则简称为“模块”</font>

<font style="color:red;font-size:18px">模块在其自身的作用域里执行，而不是在全局作用域里</font>

这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确地使用export形式之一导出它们。

相反，如果想使用其它模块导出的变量，函数，类，接口等的时候，你必须要导入它们，可以使用import形式之一。

模块是自声明的;两个模块之间的关系是通过在文件级别上使用imports和exports建立的。

如果一个文件不带有顶级的import或者export声明，那么它的内容被视为全局可见的**(因此对模块也是可见的)**

<font style="color:lightskyblue;font-size:18px">调用一个模块都要用import来调用其他模块</font>

```js
import {add as MathAdd} from './MathModule.js'
console.log(MathAdd(1,2,3));
```

<font style="color:lightskyblue;font-size:18px">每个模块都要有输出export接口</font>

```js
function add(...num){
    let sum = num.reduce((pre,next)=>{
        return pre + next;
    });
    return sum;
}
export{add};
```

<font style="color:lightskyblue;font-size:18px">单模块单文件存储</font>

![单模块单文件存储](https://img-blog.csdnimg.cn/20190304200859954.png)

## 模块导出

<font style="color:red;font-weight:900;">任何声明(比如变量，函数，类，类型别名或接口）都能够通过添加export关键字来导出。</font>

举个🌰:

![ts modules .png](https://i.loli.net/2020/12/11/Xvn8sWThk4CK9P5.png)

## 模块的重新导出

我们经常会去扩展其它模块，并且只导出那个模块的部分内容。重新导出功能并不会在当前模块导入那个模块或定义一个新的局部变量。

一个模块既可以引入也可以输出，甚至可以两者—起实现.

![imex.png](https://i.loli.net/2020/12/11/nE2FSOQ6RzVKie5.png)

基于前面图例的代码项目`tsMoudles`,我们尝试在`demo.ts`中修改一下:

```typescript
import { userMsg } from './tsMoudles/file'
console.log(userMsg);
//无法分配到“userMsg”，因为它不是变量。
userMsg = "123456"
```

无论导入导出,userMsg的类型始终被“锁死”在最初的`head.ts`类型属性里

```typescript
userMsg.age = 18//正确
```

或者一个模块可以包裹多个模块，并把他们导出的内容联合在一起通过

语法:`export * from "module"`

```typescript
export * from "./stringValidator"; // exports interface stringValidator
export * from "./LettersonlyValidator" ; // exports class LettersOnlyValidator
export * from "./ZipCodeValidator"; // exports class ZipCodeValidator
```

## 模块导入

模块的导入操作与导出一样。可以使用以下import形式之一来导入其它模块中的导出内容。

### 1.导入一个模块中的某个导出内容

```typescript
import {userMsg} from './tsMoudles/file'
```

### 2.对导入内容重命名

```typescript
import {a as userMsg} from './tsMoudles/file'
```

### 3.整个模块导入到一个变量，并通过它来访问模块的导出部分

```typescript
import * as value from "./codevalue";
let myvalue = new value.codevalue( );
```

通配符` *`的小🌰

![tsfb0590c155ebaad3.png](https://cdn.longdoer.com/2020/12/11/tsfb0590c155ebaad3.png)

## 默认导出

从前面的例子可以看出，使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。但是，用户肯定希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。

为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到export default命令，为模块指定默认输出。

```typescript
//export default.js
export default function(){
    console.log('foo')
}
```

上面代码是一个模块文件export-defau1t.js，它的默认输出是一个函数.

```typescript
//import default.js
import customName from './export_default';
customName();
```

其他模块加载该模块时， import命令可以为该匿名函数指定任意名字。

上面代码的import命令，可以用任意名称指向export-default.js输出的方法，这时就不需要知道原模块输出的函数名。需要注意的是，这时import命令后面，不使用大括号。

⚠️建议不要默认导出,因为你可能会不清楚导出的内容(命名),会加大后期维护成本

#### 默认导出优点:

方便!

`math.ts`

```ts
export default function zzy(){
    //这里函数取名为zzy,其实写不写都一样
    console.log("hello,zzy!");
    
}
```

`file.ts`

😉默认导出名字写在括号外,所以不能检查名称是否正确(前面也说到了不建议用哦)

```typescript
import 婷宝儿,{ add,userMsg } from "./math" //婷宝儿这个是随便起的,你写什么名字都行,不一定非要一样啊嘞~
婷宝儿();//调用当然也没错了鸭,不过要和你上面import定义的一样
```

图例:

[![ts89e867b8b7e64995.png](https://cdn.longdoer.com/2020/12/11/ts89e867b8b7e64995.png)](http://www.ipicbed.com/image/gut3P)

------

## 科普

我就不叫拓展咋地😎

#### export =和import = require()

commonJs和AMD的环境里都有一个exports变量，这个变量包含了一个模块的所有导出内容。

Common3s和AMD的exports都可以被赋值为一个对象，这种情况下其作用就类似于es6语法里的默认导出，即export default语法了。虽然作用相似，但是 export default语法并不能兼容CommonjSAMD的exports。

<font style="color:tomato;">为了支持CommonJs和AMD的exports，Typescript提供了export =语法。</font>

export=语法定义一个模块的导出对象。这里的对象—词指的是类，接口，命名空间，函数或枚举。

若使用export =导出一个模块，则必须使用Typescript的特定语法`import module =require( "module")`来导入此模块。



#### 生成模块代码

根据编译时指定的模块目标参数，编译器会生成相应的供Node.js (CommonJS)，Require.js(AMD)，UMD，SystemJS或ECMAScript 2015 native modules (ES6)模块加载系统使用的代码。



为了编译，我们必需要在命令行上指定一个模块目标。对于Node.js来说，使用--module commonjs;对于Require.js来说，使用--module amd

```typescript
tsc --module commonjs Test.ts(文件名)
```

![x0b84397d91c17550.png](https://cdn.longdoer.com/2020/12/11/x0b84397d91c17550.png)

------

## NEXT:命名空间

> “内部模块”现在称做“命名空间”;“外部模块”现在则简称为“模块”;

我们定义几个简单的字符串验证器，假设我们会使用它们来验证表单里的用户输入或验证外部数据。

<font style="color:deepskyblue;">所有的验证器都放在一个文件里</font>

随着更多验证器的加入，我们需要─种手段来组织代码，以便于在记录它们类型的同时还不用担心与其它对象产生命名冲突。因此，我们把验证器包裹到一个命名空间内，而不是把它们放在全局命名空间下。这种写法，比起把所有的变量或是函数放在—个对象的属性里更加简单.

