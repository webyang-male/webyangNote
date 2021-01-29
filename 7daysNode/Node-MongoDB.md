---
title: Node-MongoDB
date: 2020-12-25 09:59:29
tags:  MongoDB
categories: Node
description: MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。
---

#### 简介

官网：[https://www.mongodb.com/]
	   手册：[https://docs.mongodb.com/manual/]
	   中文手册: [https://www.mongodb.org.cn/]
						[https://docs.mongoing.com/]

传统的数据库都是结构性数据库，如MySQL、SQL Server、Oracle、Access等数据库。有行和列的概念，数据有关系并且数据不是散的。每个表中，都有明确的字段，每行记录，都有这些字段，不能有的行有，有的行没有。

MongoDB是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

#### 安装

1. 打开下载链接：
   [https://www.mongodb.com/try/download/community]
   (如果是32位的，用这个地址:[http://dl.mongodb.org/dl/win32/x86_64])

2. 选择对应的下载版本,下载msi程序

3. 下载完毕后进行安装，默认或者自定义都可以（建议默认）

4. 安装的过程中注意不要勾选‘Install MongoDB Compass’。MongoDB Compass是一个图形界面管理工具，不安装没有问题的，我们用<a href='https://robomongo.org/'>Robo 3T</a>这个图形界面管理工具。

   具体安装步骤自行参考网上教程

#### 详细使用

第一步需要连接数据库，建议大家配置好环境变量。然后打开cmd，执行mongo命令

| 命令                   | 作用                 |
| ---------------------- | -------------------- |
| mongo                  | 使用数据库           |
| show dbs               | 列出所有数据库       |
| show collections       | 查看全部集合(表)     |
| use 数据库名字         | 使用和新建数据库     |
| db                     | 查看当前操作的数据库 |
| db.数据库名称.insert() | 插入数据             |
| db.数据库库名称.find() | 查找数据             |
| db.数据库名称.update() | 修改数据             |
| db.数据库名称.remove() | 删除数据             |
| db.数据库名称.drop()   | 删除集合             |
| db.dropDatabase()      | 删除数据库           |

**关系比较符：**
   小于：$lt
   小于或等于：$lte
   大于：$gt
   大于或等于：$gte
   不等于：$ne
   属于：$in

**等于** : 在MongoDB中什么字段等于什么值其实就是 " : " 来搞定 比如 "name" : "jack"

```json
"name" : "jack"
```

#### 启动和关闭数据库

启动

```shell
 # mongodb默认使用执行mongod命令所处盘符根目录下的/data/db 作为自己的数据存储目录
 # 在第一次执行该命令之前先自己手动新建一个/data/db
mongod
```

如果想要修改默认的数据存储目录，可以

```shell
mongod --dbpath=数据存储目录路径
```

停止

```shell
#在开启服务的控制台,直接ctrl+c即可停止
#直接关闭开启服务的控制台
#输入 exit 命令退出关闭
```

#### 初识mongoose

> Mongoose是在node.js异步环境下对mongodb进行便捷操作的对象模型工具

[Mongoose介绍和入门]: https://www.cnblogs.com/chris-oil/p/12593684.html

**案例目录**

![mongoose.png](https://s1.imagehub.cc/images/2020/12/25/mongoose.png)

```js
var mongoose = require('mongoose');

// 连接 MongoDB 数据库
mongoose.connect('mongodb://localhost/test', { useMongoClient: true });

mongoose.Promise = global.Promise;

// 创建一个模型
// 就是在设计数据库
// MongoDB 是动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose 这个包就可以让你的设计编写过程变的非常的简单
var Cat = mongoose.model('Cat', { name: String });

for (var i = 0; i < 100; i++) {
  // 实例化一个 Cat
  var kitty = new Cat({ name: '喵喵' + i });

  // 持久化保存 kitty 实例
  kitty.save(function (err) {
    if (err) {
      console.log(err);
    } else {
      console.log('meow');
    }
  });
}

```

![mongoose1-1.png](https://s1.imagehub.cc/images/2020/12/25/mongoose1-1.png)

#### MongoDB数据库概念

- 可以有多个数据库
- 一个数据库中可以有多个集合（表)
- 一个集合中可以有多个文档（表记录)
- 文档结构很灵活，没有任何限制
- MongoDB非常灵活，不需要像 MySQL一样先创建数据库、表、设计表
  - 需要插入数据的时候，只需要指定往哪个数据库的哪个集合操作就可以了
  - —切都由MongoDB自动完成建库建表

##### MongoDB支持哪些数据类型

```js
String
Integer
Double
Boolean
Object
Object ID
Arrays
Min/Max Keys
Datetime
Code （用于在文档中存储 JavaScript 代码）
Regular Expression等
```

#### Mongoose

`npm`安装Mongoose:

```shell
$ npm install mongoose
```

```js
var mongoose = require('mongoose')

var Schema = mongoose.Schema

// 1. 连接数据库
// 指定连接的数据库不需要存在，当你插入第一条数据之后就会自动被创建出来
mongoose.connect('mongodb://localhost/itcast')

// 2. 设计文档结构（表结构）
// 字段名称就是表结构中的属性名称
// 约束的目的是为了保证数据的完整性，不要有脏数据
var userSchema = new Schema({
  username: {
    type: String,
    required: true // 必须有
  },
  password: {
    type: String,
    required: true
  },
  email: {
    type: String
  }
})

// 3. 将文档结构发布为模型
//    mongoose.model 方法就是用来将一个架构发布为 model
//    第一个参数：传入一个大写名词单数字符串用来表示你的数据库名称
//                 mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
//                 例如这里的 User 最终会变为 users 集合名称
//    第二个参数：架构 Schema
//   
//    返回值：模型构造函数
var User = mongoose.model('User', userSchema)


// 4. 当我们有了模型构造函数之后，就可以使用这个构造函数对 users 集合中的数据为所欲为了（增删改查）
// **********************
// #region /新增数据
// **********************
// var admin = new User({
//   username: 'zs',
//   password: '123456',
//   email: 'admin@admin.com'
// })

// admin.save(function (err, ret) {
//   if (err) {
//     console.log('保存失败')
//   } else {
//     console.log('保存成功')
//     console.log(ret)
//   }
// })
// **********************
// #endregion /新增数据
// **********************




// **********************
// #region /查询数据
// **********************
// User.find(function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })

// User.find({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })

// User.findOne({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('查询失败')
//   } else {
//     console.log(ret)
//   }
// })
// **********************
// #endregion /查询数据
// **********************



// **********************
// #region /删除数据
// **********************
// User.remove({
//   username: 'zs'
// }, function (err, ret) {
//   if (err) {
//     console.log('删除失败')
//   } else {
//     console.log('删除成功')
//     console.log(ret)
//   }
// })
// **********************
// #endregion /删除数据
// **********************


// **********************
// #region /更新数据
// **********************
// User.findByIdAndUpdate('5a001b23d219eb00c8581184', {
//   password: '123'
// }, function (err, ret) {
//   if (err) {
//     console.log('更新失败')
//   } else {
//     console.log('更新成功')
//   }
// })
// **********************
// #endregion /更新数据
// **********************

```

#### [mongoose](http://www.mongoosejs.net/)官方文档

http://www.mongoosejs.net/docs/api.html#Model

------

#### Node操作MySQL数据库

安装

```shell
npm i --save mysql
```

```javascript
var mysql = require('mysql');

// 1. 创建连接
var connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'root',
  database: 'students' //数据库名
});

// 2. 连接数据库 打开冰箱门
connection.connect();

//插入一条数据显示效果
//表字段--id username password
// connection.query('INSERT INTO users VALUES(NULL, "admin", "123456")', function (error, results, fields) {
//   if (error) throw error;
//   console.log('The solution is: ', results);
// });

// 3. 执行数据操作 把大象放到冰箱
connection.query('SELECT * FROM `users`', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results);
});


// 4. 关闭连接 关闭冰箱门
connection.end();

```

