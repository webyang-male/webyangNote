---
title: tsconfig
date: 2020-12-13 21:25:42
tags: js/typescript
categories: Javascript
---

## 命名空间和模块的陷阱

### 一．对模块使用   <reference>

一个常见的错误是使用`<reference>`引用模块文件，引用模块文件应该使用`import`

要理解这之间的区别，我们首先应该弄清编译器是如何根据import路径（例如，import x from "...";或import x = require(" ...")里面的...,等等)来定位模块的类型信息。

1. 首先尝试去查找相应路径下的.ts文件
2. 再查找.tsx文件(jsx语法的文件，也是一种JavaScript的超集,现在广泛运用于React中)
3. 再查找.d.ts文件(当你安装 TypeScript时，会顺带安装 lib.d.ts等声明文件。此文件包含了JavaScript运行时以及DOM中存在各种常见的环境声明。node中也有这种类似的库文件)
4. 如果这些文件都找不到，编译器会查找外部模块声明(其他模块的输出项).

### 二．不必要的命名空间

如果你想把命名空间转换为模块，它可能会像下面的示例一样:

shapes.ts

```typescript
export namespace Shapes{
    export class Triangle{/* ... */}
    export class Square{/* ... */}
}
```

test.ts

```typescript
import * as shapes from "./moudle/shapes";
let t = new shapes.Shapes.Triangle();
console.log(shapes);
```

TypeScript里模块的一个特点是**不同的模块永远也不会在相同的作用域内使用相同的名字**。因为使用模块的人会为它们命名，**所以完全没有必要把导出的符号包裹在一个命名空间里**。这样我们还是要去使用到原模块文件里面的对象名称，这样就变的格外的麻烦

下面是改进的🌰:

shapes.ts

```typescript
  export class Triangle{/* ... */}
  export class Square{/* ... */}
```

test.ts

```typescript
import * as shapes from "./moudle/shapes";
let t = new shapes.Shapes.Triangle();
console.log(shapes);
```

注意:不应该对模块使用命名空间，使用命名空间是为了提供逻辑分组和避免命名冲突。模块文件本身已经是一个逻辑分组，并且它的名字是由导入这个模块的代码指定，所以没有必要为导出的对象增加额外的模块层。

## tsconfig.json概述

如果一个目录下存在一个tsconfig.json文件，那么它意味着这个目录是TypeScript项目的根目录。

```typescript
├── moudle
│ ├── shapes.ts
├── test.ts
├── tsconfig.json
```

`tsconfig.json`文件中指定了用来编译这个项目的根文件和编译选项。一个项目可以通过以下方式之一来编译:

1. 不带任何输入文件的情况下调用tsc，编译器会从当前目录开始去查找`tsconfig. json`文件，逐级<font style="color:red;">向上搜索父目录。</font>
2. 不带任何输入文件的情况下调用tsc，且使用命令行参数--project(或-p）指定一个包含`tsconfig.json`文件的目录。
3. 当命令行上指定了输入文件时，`tsconfig.json`文件会被忽略。"files"指定一个包含相对或绝对文件路径的列表。

[![tsfb293d008985ca7b.md.png](https://cdn.longdoer.com/2020/12/13/tsfb293d008985ca7b.png)](http://www.ipicbed.com/image/gbgdg)

![tsb5050eb752c625a7.png](https://cdn.longdoer.com/2020/12/14/tsb5050eb752c625a7.png)

## tsconfig.json细节

“include(包括)”和exclude(排除)"属性指定一个文件glob匹配模式列表。支持的glob通配符有:

1.*匹配o或多个字符(不包括目录分隔符)

2.?匹配一个任意字符(不包括目录分隔符)

3.**/递归匹配任意子目录

![ts_includf8eeef00fb0128a0.png](https://cdn.longdoer.com/2020/12/14/ts_includf8eeef00fb0128a0.png)

![exc_ts33e020f19a3b1623.png](https://cdn.longdoer.com/2020/12/14/exc_ts33e020f19a3b1623.png)

1. 如果一个glob模式里的某部分只包含`*`或`.*`，那么仅有<font style="color:red;">支持的文件扩展名类型被包含在内</font>(比如默认.ts,.tsx,和.d.ts,如果allowJs设置能true还包含.js和.jsx).
2. 如果"files"和"include"都没有被指定，编译器默认包含当前目录和子目录下所有的TypeScript文件(.ts，.d.ts和.tsx)，排除在"exclude"里指定的文件。使用“outDir"指定的目录下的文件永远会被编译器排除，除非你明确地使用"files"将其包含进来（这时就算用exclude指定也没用).

### tsconfig.json之extends继承配置

tsconfig.json文件可以利用extends属性从另一个配置文件里继承配置

extends是tsconfig.json文件里的顶级属性（与compileroptions，files，include，和exclude一样)。extends的值是一个字符串，包含指向另一个要继承文件的路径。

在原文件里的配置先被加载，然后被来至继承文件里的配置重写。如果发现循环引用，则会报错。来至所继承配置文件的files，include和exclude覆盖源配置文件的属性。配置文件里的相对路径在解析时相对于它所在的文件。

![tsconf_-1bdfe6659d2cf5aa8.png](https://cdn.longdoer.com/2020/12/14/tsconf_-1bdfe6659d2cf5aa8.png)

### tsconfig.json其他细节

- 使用"include"引入的文件可以使用"exclude"属性过滤。然而，通过“files"属性明确指定的文件却总是会被包含在内，不管"exclude"如何设置。如果没有特殊指定,<font style="color:red;">“exclude"默认情况下会排除node_modules，bower_components， jspm_packages和`<outDir>`目录</font>。
- 任何被"files"或"include"<font style="color:red;">指定的文件所引用的文件</font>也会被包含进来。A.ts引用了B.ts，因此B.ts不能被排除，除非引用它的A.ts在"exclude"列表中。
- 需要注意编译器不会去引入那些可能做为输出的文件;比如，假设我们包含了index.ts，那么index.d.ts和index.js会被排除在外。通常来讲，不推荐只有扩展名的不同来区分同目录下的文件。
- tsconfig.json文件可以是个空文件，但编译会直接报错。
- 在命令行上指定的编译选项会覆盖在tsconfig.json文件里的相应选项。

## tsconfig.json编译选项

#### (@types, typeRoots和types)

默认所有可见的“@types”包会在编译过程中被包含进来（这个包是指那些大型的插件或是库)

node_modules/@types文件夹下以及它们子文件夹下的所有包都是可见的;也就是说，`./node_modules/@types/，../node_modules/@types/和../ ../node_modules/@types/`等等。

如果指定了typeRoots(包的根目录)，只有typeRoots下面的包才会被包含进来。

```json
{
    "compilerOptions": {
        "typeRoots": ["./typings"]
    },
}
```

这个配置文件会包含所有`./typings`下面的包，而不包含`./ node_modules/@types`里面的包。

如果指定了types，只有被列出来的包才会被包含进来。

```json
{
    "compilerOptions": {
        "types": ["node","express","koa"]
    },
}
```

这个tsconfig.json文件将仅会包含`./node_modules/@types/node`,

`./node_modules/@types/koa `和` ./node_modules/@types/express/@types/ `,`node_modules/@types/*`里面的**其它包**不会被引入进来。

<font style="color:tomato;">指定"types":[]来禁用自动引入@types包。</font>

注意，自动引入只在你使用了全局的声明(相反于模块）时是重要的。如果你使用import "foo"语句，Typescript仍然会查找node_modules和node_modules/@types文件夹来获取foo包。

## tsconfig.json的编译器命令

![ts_command4ed545e1a4a41c14.png](https://cdn.longdoer.com/2020/12/14/ts_command4ed545e1a4a41c14.png)

 -- lib：编译时要使用的库

注意:如果--lib没有指定默认注入的库的列表。默认注入的库为:
					：ES5: DOM,ES5,ScriptHost
    				：ES6: DOM,ES6,DOM.Iterable,ScriptHost

![ts_command-183e41d4de5aaefcb.png](https://cdn.longdoer.com/2020/12/14/ts_command-183e41d4de5aaefcb.png)

![ts_command-24b03c06461aa22a6.png](https://cdn.longdoer.com/2020/12/14/ts_command-24b03c06461aa22a6.png)

![ts_command-31bbb6473515e373d.png](https://cdn.longdoer.com/2020/12/14/ts_command-31bbb6473515e373d.png)

![ts_command-44fcb5703aa76529c.png](https://cdn.longdoer.com/2020/12/14/ts_command-44fcb5703aa76529c.png)