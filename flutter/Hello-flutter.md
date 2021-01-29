---
title: Hello flutter
date: 2020-12-26 23:11:35
tags: flutter
categories: flutter
description: 🌱flutter学习之路开篇
---

<svg t="1608995651198" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1959" width="32" height="32"><path d="M610.730667 0L98.133333 512 256 669.866667 925.184 0.512h-313.898667L610.730667 0z m0.597333 472.405333l-276.096 275.498667 276.053333 276.053333H925.866667l-275.626667-275.968 275.626667-275.626666h-314.496z" p-id="1960" fill="#1296db"></path></svg>

#### 跨平台框架的发展历史

![apphistory.png](https://s1.imagehub.cc/images/2020/12/30/apphistory.png)

图片原文:https://blog.csdn.net/pkx1993/article/details/81221256

##### 3个时代跨平台框架对比

![3.png](https://s1.imagehub.cc/images/2020/12/30/3.png)

##### 其他文章:

[Flutter概述、原理]: https://www.jianshu.com/p/17948001ce8a
[移动开发的跨平台技术发展史 | 技术头条]: https://blog.csdn.net/csdnnews/article/details/89629327

------

#### Flutter常用命令

##### Flutter SDK分支

```shell
flutter channel
```

##### 升级flutter SDK和依赖包

```shell
#升级（此命令会同时更新 Flutter SDK 和你的 Flutter 项目依赖包）
flutter upgrade

#获取依赖包（只更新项目依赖包，不包括 Flutter SDK）
flutter packages get

#升级依赖包（只更新项目依赖包，不包括 Flutter SDK）
flutter packages upgrade
```

##### 查看环境依赖(是否安装成功)

```shell
flutter doctor

#查看详细信息
flutter doctor -v	
```

##### 分析项目代码

```shell
flutter analyze
```

##### 其他指令

| 常用命令       | 含义                                    |
| -------------- | --------------------------------------- |
| --version      | 查看Flutter版本                         |
| -h或者--help   | 打印所有命令行用法信息                  |
| analyze        | 分析项目的Dart代码。                    |
| build          | Flutter构建命令。                       |
| channel        | 列表或开关Flutter通道。                 |
| clean          | 删除构建/目录。                         |
| config         | 配置Flutter设置。                       |
| create         | 创建一个新的Flutter项目。               |
| devices        | 列出所有连接的设备。                    |
| drive          | 为当前项目运行Flutter驱动程序测试。     |
| format         | 格式一个或多个Dart文件。                |
| fuchsia_reload | 在Fuchsia上进行热重载。                 |
| help           | 显示帮助信息的Flutter。                 |
| install        | 在附加设备上安装Flutter应用程序。       |
| logs           | 显示用于运行Flutter应用程序的日志输出。 |
| packages       | 命令用于管理Flutter包。                 |
| precache       | 填充了Flutter工具的二进制工件缓存。     |
| run            | 在附加设备上运行你的Flutter应用程序。   |
| screenshot     | 从一个连接的设备截图。                  |
| stop           | 停止在附加设备上的Flutter应用。         |
| test           | 对当前项目的Flutter单元测试。           |
| trace          | 开始并停止跟踪运行的Flutter应用程序。   |

#### IDE插件配置

##### 安装Android Studio

- [Android Studio](https://developer.android.com/studio/index.html), 3.0或更高版本.

或者，您也可以使用IntelliJ：

- [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/), version 2017.1或更高版本.
- [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/download/), version 2017.1 或更高版本.

##### 安装Flutter和Dart插件

需要安装两个插件:

- `Flutter`插件： 支持Flutter开发工作流 (运行、调试、热重载等).
- `Dart`插件： 提供代码分析 (输入代码时进行验证、代码补全等).

要安装这些:

1. 启动Android Studio.
2. 打开插件首选项 (**Preferences>Plugins** on macOS, **File>Settings>Plugins** on Windows & Linux).
3. 选择 **Browse repositories…**, 选择 Flutter 插件并点击 `install`.
4. 重启Android Studio后插件生效.

*VS Code:* 轻量级编辑器，支持Flutter运行和调试.

##### 安装 VS Code

- [VS Code](https://code.visualstudio.com/), 安装1.20.1或更高版本.

##### 安装Flutter插件

1. 启动 VS Code
2. 调用 **View>Command Palette…**
3. 输入 ‘install’, 然后选择 **Extensions: Install Extension** action
4. 在搜索框输入 `flutter` , 在搜索结果列表中选择 ‘Flutter’, 然后点击 **Install**
5. 选择 ‘OK’ 重新启动 VS Code

##### 通过Flutter Doctor验证设置

1. 调用 **View>Command Palette…**
2. 输入 ‘doctor’, 然后选择 **‘Flutter: Run Flutter Doctor’** action
3. 查看“OUTPUT”窗口中的输出是否有问题



#### 避坑博客

[Android安装Flutter插件的坑]: https://blog.csdn.net/u014736095/article/details/89377692

```markdown
Android studio 安装 flutter 和Dart 插件时， 最好是到插件官网下载对应的插件版本。Android studio 3.1的版本对应的分别是 。 一定要保持版本号一致，否则会导致磁盘安装也是失败的。

✏️插件官网地址：http://plugins.jetbrains.com/androidstudio
```

[提示Flutter plugin not installed，实际已安装插件]: https://blog.csdn.net/kaixinlaok/article/details/110522275



#### 结束语

不管未来有新的框架出现、还是老框架更新迭代，我们需要做的是掌握核心原理才能真正利于不败之地。

对于实际项目:

- 中短期项目,建议使用React-Native
- 长期项目，建议使用Flutter

