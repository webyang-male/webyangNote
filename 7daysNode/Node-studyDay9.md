---
title: Node-studyDay9
date: 2020-12-24 11:29:49
tags: Node
categories: Node
description: npm相关简单学习~
---

### NPM

> node package manager

### Package.json

建议每一个项目都要有一个`package.json`文件（包描述文件，就像产品的说明书一样)，给人踏实的感觉

这个文件可以通过`npm init` 的方式来自动初始化出来。

对于咱们目前来讲，最有用的是`package.json`文件中的[dependencies]选项，可以用来帮我们保存第三方包的依赖信息。

如果你的`node_modules`删除了也不用担心，我们只需要:`npm install`就会自动把(package .json)中的`dependencies `中所有的依赖项都下载回来。

✏️建议执行[npm install包名的的时候都加上`--save`这个选项，目的是用来保存依赖项信息

快速创建:

```shell
npm init -y
```

使用一下命令能安装所有依赖

```shell
npm install
```

package.json介绍官网：[http://docs.npmjs.com/files/package.json]

### 使用方法

#### 1.首先在项目中打开终端输入指令`npm init`或者`npm init -y `     4

 -y 的意思是默认全部同意

作用是在初始化的时候帮助你生成一个package. json的文件,永远不要删除这个文件

`注意事项`:项目运行中不要删除`package.json`这个文件，这个文件会记录当前项目的各种依赖

#### 2.安装包

```shell
npm install  包名字  (可以@加版本) -参数(S, D, g) 	
```

简写:   npm   i   包名    -参数

安装后会在项目目录中生产node modules文件夹

参数:

- -S --save 安装在当前项目下，生产环境(不仅仅在开发过程中需要，上线之 后也需要)

- -D --save–dev安装在当前项目下，开发环境(写代码时候用，上线不用)

- -g   安装在全局，般是在node的安装目录

#### 3.npm换源

npm的服务器在国外，可能会出现网络波动问题可以将他换源成淘宝镜像npm

换淘宝镜像指令: npm config set registry  https:// registry. npm. taobao. org

换回原来的服务器: npm config set registry https://registry.npmjs.org/(需要用到publish就要换回去)

#### 4.卸载:

```shell
	npm uninstall 包名- 参数

	npm   un 包名-参数

```

#### 5.更新:

```shell
npm update 包名
```

#### 6.查看项目的包列表

```sh
npm list
```

#### 7.查看全局包列表

```shell
	npm list -g
```

### 常用NPM命令扩展

```cmd
// 安装淘宝镜像 解决被墙,网速卡问题
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```cmd
npm init | npm init -y创建项目依赖文件package.json
```

```package.json
"dependencies": {
    "gulp": "^3.9.1"
}
// package.json中安装的依赖包前面的符号代表的意思
// ~匹配最近的小版本依赖包，比如~1.1.2会匹配所有1.1.x版本，但不包括1.2.0
// ^匹配最新的大版本依赖包，比如^3.4.5会匹配所有3.x.x的包，包括3.5.0，但不包括4.0.0
// *安装最新版本的依赖包
```

- 1.安装模块

```cmd
npm install   // 项目中存在package.json文件并且写入了依赖配置时，会下载所有的依赖包
```

```cmd
npm install 包名   // 本地安装依赖包
```

```cmd
npm install 包名 -g   // 全局安装依赖包
```

```cmd
npm install 包名@版本号   // 安装指定版本
```

```cmd
npm install 包名 --save/-S   // 安装包信息将加入到dependencies(生产阶段的依赖)
```

```cmd
npm install 包名 --save-dev/-D   // 安装包信息将加入到devDependencies(开发阶段的依赖)
```

- 2.卸载模块

```cmd
npm uninstall [包名|包名@版本] [-S|--save|-D|--save-dev]
```

删除的同时也会把依赖信息也去除

npm un -S 包名

```shell
npm uninstall --save包名 
```

- 3.更新模块

```cmd
npm update [-g] [包名]

npm  -g  update  包名  #  全局更新
npm  update  包名  #  本地更新
```

- 4.清理本地缓存

```cmd
npm cache clean
```

- 5.查看指定命令的使用帮助

npm 命令 --help

```shell
//例如我忘记了uninstall 命令的简写了，这个时候，可以输入
npm uninstall --help  来查看使用帮助
```

#### npm命令博客推荐

https://www.cnblogs.com/itlkNote/p/6830682.html