---
title: 模块解析机制
date: 2020-12-14 10:07:07
tags: js/typescript
categories: Javascript
---

## 模块解析策略之Node策略

<font style="color:skyblue;">非相对模块名的解析是个完全不同的过程</font>。Node会在一个特殊的文件夹node_modules里查找你的模块(我们在node环境下用npm安装组件时，会有选项提示是当前项目安装还是全局安装，这个不同的选项就会决定插件会存储在用户电脑的哪个文件里)。

还是用上面例子，但假设

/root/src/ moduleA.js里使用的是非相对路径导入`var x = require(" moduleB”);`。Node则会以右侧的顺序去解析moduleB，直到有一个匹配上。

node_modules可能与当前文件在同一级目录下，或者在上层目录里。Node会向上级目录遍历，查找每个node_modules直到它找到要加载的模块。

```typescript
/root/src/node_modules/moduleB.js
/root/src/node_modules/moduleB/package.json(如果指定了 "main"属性)
/root/src/node_modules/moduleB/index.js

/root/node_modules/moduleB.js(上跳了一级)
/root/node_modules/moduleB/package.json(如果指定了 "main"属性)
/root/ node_modules/moduleB/index.js

/node_modules/moduleB.js（上跳了一级)
/node_modules/moduleB/package.json(如果指定了"main"属性)
/node_modules/moduleB/index.js
```

## 模块解析策略之TypeScript模块解析策略(相对)

Typescript是模仿Node.js运行时的解析策略来在编译阶段定位模块定义文件。

因此,TypeScript在Node解析逻辑基础上增加了TypeScript源文件的扩展名（.ts，.tsx和.d.ts)。同时，Typescript在package.json里使用字段"types"来表示类似"main"的意义– 编译器会使用它来找到要使用的"main"定义文件。

```typescript
比如，有一个导入语句import { b }from "./moduleB"在/root/src/moduleA. ts.里，会以下面的流程来定位"./moduleB":
/root/src/moduleB.ts
/root/src/moduleB.tsx
/root/src/moduleB.d.ts
/root/src/moduleB/package.json(如果指定了"types"属性)
/root/src/moduleB /index.ts
/root/src/moduleB /index.tsx
/root/src/moduleB/index.d.ts
```

## 模块解析策略之TypeScript模块解析策略(非相对)

类似地，非相对的导入会遵循Node.js的解析逻辑，首先查找文件，然后是合适的文件夹。

因此/root/src/moduleA.ts文件里的import { b }frommoduleB”会以右侧的查找顺序解析:

![ts-1e845a66f90e777e8.png](https://cdn.longdoer.com/2020/12/14/ts-1e845a66f90e777e8.png)

不要被这里步骤的数量吓到- Typescript只是在步骤（8）和（15)向上跳了两次目录。这并不比Node.js里的流程复杂。

## 模块解析策略之附加的模块解析标记

**有时工程源码结构与输出结构不同**。通常是要经过一系统的构建步骤最后生成输出。它们包括将.ts编译成.js，将不同位置的依赖拷贝至一个输出位置。最终结果就是运行时的模块名与包含它们声明的源文件里的模块名不同。或者最终输出文件里的模块路径与编译时的源文件路径不同了。

Typescript编译器有一些额外的标记用来通知编译器在源码编译成最终输出的过程中都发生了哪个转换

有一点要特别注意的是编译器不会进行这些转换操作;它只是利用这些信息来指导模块的导入。

## 模块解析策略之附加的模块解析标记(BaseURL)

在利用AMD模块加载器的应用里使用baseUrl是常见做法，它要求在运行时模块都被放到了一个文件夹里。这些模块的源码可以在不同的目录下，但是构建脚本会将它们集中到一起。

<font style="color:tomato;">设置baseUrl来告诉编译器到哪里去查找模块。所有非相对模块导入都会被当做相对于baseurl。</font>(相当于手动设立根目录)

baseUrl的值由以下两者之一决定:

1. 命令行中baseUrl的值(如果给定的路径是相对的，那么将相对于当前路径进行计算)
2. tsconfig.json’里的baseurl属性(如果给定的路径是相对的，那么将相对于'tsconfig.json’路径进行计算)

注意相对模块的导入不会被设置的baseUr1所影响，因为它们总是相对于导入它们的文件。

## 模块解析策略之附加的模块解析标记(路径映射)

有时模块不是直接放在baseUrl下面。比如，充分"jquery"模块地导入，在运行时可能被解释为`"node_modules/jquery/dist/jquery.slim.min.js"`。

Typescript编译器通过使用tsconfig.json文件里的"paths"来支持这样的声明映射

```typescript
{
    "compilerOptions": {
        "baseUrl": ".",//指定的paths的根目录
        "paths": {
            "jquery": [
                "node_modules/jquery/dist/jquery"
            ] //此处映射是相当于"baseUrl"
        }
    },
}
```

请注意"paths"是相对于"baseUrl"进行解析。如果"baseUrl"被设置成了除"."外的其它值，比如tsconfig.json所在的目录，那么映射必须要做相应的改变。如果你在上例中设置了“baseUrl": "./src"，那么jquery应该映射到`"../node_modules/jquery/dist/jquery"`。

通过"paths"我们还可以指定复杂的映射，包括指定多个回退位置。

假设在一个工程配置里，有一些模块位于一处，而其它的则在另个的位置。构建过程会将它们集中至一处。工程结构可能如下:

```markdown
projectRoot
├── folderl
│   ├── file1.ts (imports 'folder1/file2' and 'folder2/file3')
│   └── file2.ts
├── generated
│   ├── folder1
│   ├── folder2
│     └── file3.ts
│  
└── tsconfig.json
```

它告诉编译器所有匹配"*”(所有的值)模式的模块导入会在以下两个位置查找

1." * ":表示名字不发生改变，所以映射为`<moduleName> =><baseUrl> /<moduleName>`

2. `"generated/*"`表示模块名添加了“generated”前缀，所以映射为`<moduleName> => <baseUr1>/generated/<moduleName>`

````typescript
{
  "compilerOptions":{
    "baseUrl":".",
      "paths":{
        "*":{
          "*",
            "generated/*"
        }
     }
  }
}
````

相应的`tsconfig.json`文件

##### 按照这个逻辑，编译器将会如下尝试解析这两个导入:

###### 导入'folder1/file2'

1. 匹配'*'模式且通配符捕获到整个名字。
2. 尝试列表里的第一个替换:'*'-> folder1/file2。
3. 替换结果为非相对名–与baseurl合并->projectRoot/folder1/file2.ts。
4. 文件存在,完成。

##### 导入'folder2/file3'

1. 匹配"*'模式且通配符捕获到整个名字。
2. 尝试列表里的第一个替换:'*'-> folder2/file
3. 替换结果为非相对名-与baseUrl合并->projectRoot/folder2/file3.ts。
4. 文件不存在，跳到第二个替换。
5. 第二个替换: 'generated/*' -> generated/folder2/file3。
6. 替换结果为非相对名-与baseurl合并->projectRoot/generated/folder2/file3.ts
7. 文件存在。完成。



## 模块解析策略之附加的模块解析标记(利用rootDirs指定虚拟目录)

有时多个目录下的工程源文件在编译时会进行合并放在某个输出目录下。这可以看做一些源目录创建了一个“虚拟”目录。

利用rootDirs，可以告诉编译器生成这个虚拟目录的roots;因此编译器可以在“虚拟”目录下解析相对模块导入，就好像它们被合并在了一起一样。

```typescript
src
└── view
   └── view.ts (imports './templatel')
   └── view2.ts 
generated
└── templates
   └── views 
   └── templates.ts (imports './view2')

```

**比如，有上面的工程结构**

src/views里的文件是用于控制uI的用户代码。generated/templates是UI模版，在构建时通过模版生成器自动生成。构建中的一步会将/src/views和/generated/templates/views的输出拷贝到同一个目录下.在运行时，视图可以假设它的模版与它同在一个目录下,因此可以使用相对导入"./template"。

可以使用"rootDirs"来告诉编译器。“rootDirs"指定了一个roots列表，列表里的内容会在运行时被合并。因此，针对这个例子， tsconfig.json如下:

```typescript
{
  "compilerOptions":{
    "rootDirs":{
      "src/views",
        "generated/templates/views"
    }
}
```

每当编译器在某一rootDirs的子目录下发现了相对模块导入，它就会尝试从每一个rootDirs中导入。

rootDirs的灵活性不仅仅局限于其指定了要在逻辑上合并的物理目录列表。<font style="color:tomato;">它提供的数组可以包含任意数量的任何名字的目录，不论它们是否存在。</font>这允许编译器以类型安全的方式处理复杂捆绑(bundles)和运行时的特性，比如条件引入和工程特定的加载器插件。



## 模块解析策略之附加的模块解析标记(跟踪模块解析)

编译器在解析模块时可能访问当前文件夹外的文件。这会导致很难诊断模块为什么没有被解析，或解析到了错误的位置。通过`--traceResolution`启用编译器的模块解析跟踪，它会告诉我们在模块解析过程中发生了什么。

```typescript
import * as ts from " ./ moudle/Math";
{
   enum test{a,b,c)
}
```

使用--traceResolution调用编译器。

```typescript
> tsc --traceResolution
```

<FONT STYLE="color:tomato;font-size:18px;">需要留意的地方</FONT>

1．导入的名字及位置

```typescript
====== Resolving module 'typescript' from 'src/app.ts'. ======
```

2．编译器使用的策略

```typescript
Module resolution kind is not specified，using 'NodeJs'.
```

3．最终结果

```typescript
======== Module name 'typescript' was successfully resolved to'node_modules/typescript/lib/types.cnipt..d.ts.'. ========
```

## 模块解析策略之附加的模块解析标记(使用--noResolve)

正常来讲编译器会在开始编译之前解析模块导入。每当它成功地解析了对一个文件 import，这个文件被会加到一个文件列表里，以供编译器稍后处理。

`--noResolve`编译选项告诉编译器:不要添加任何不是在命令行上传入的文件到编译列表。编译器仍然会尝试解析模块，但是只要没有指定这个文件，那么它就不会被包含在内。

`test.ts`

```typescript
import * as A from "moduleA"// oK, moduLeA passed on the command-Line
import * as B from "moduleB"// Error TS02: Cannot find module 'moduLeB '.
```

使用--noResolve编译app.ts:

```typescript
tsc app.ts moduleA.ts --noResolve
```

> 可能正确找到moduleA，因为它在命令行上指定了。
>
> 找不到moduleB，因为没有在命令行上传递。

## 模块解析策略之附加的模块解析标记(注意点)

<FONT STYLE="color:deepskyblue;font-size:18px;">为什么在exclude列表里的模块还会被编译器使用</font>

⚠️`tsconfig.json`将文件夹转变一个“工程”，如果不指定任何“exclude”或“files”，文件夹里的所有文件包括tsconfig.json和所有的子目录都会在编译列表里。如果你想利用“exclude”排除某些文件，甚至你想指定所有要编译的文件列表，请使用“files”。

有些是被`tsconfig.json`自动加入的。它不会涉及到上面讨论的模块解析。如果编译器识别出一个文件是模块导入目标(比如moduleA被排除了，但是moduleB用import 导入了这个模块)，它就会加到编译列表里，不管它是否被排除了。

因此，要从编译列表中排除一个文件，你需要在排除它的同时，还要排除所有对它进行import或使用了`/// <reference path="...”/>`指令的文件。