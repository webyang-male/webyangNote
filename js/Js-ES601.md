## ES6

<font style="color:gold;">let、const </font>

---2020/04/09

------



### 一. let 命令

#### 1. ES5 的变量定义

定义变量(声明变量)

```js
var a = 12;
```



##### 1.1 var 的变量的特性

1. 会发生变量提升

   ```js
   var a = 12;
   function fn(){
       alert(a);  // undefined
       var a = 5;
   }
   fn()
   ```


1. ES5 只用全局和函数内部

   ```js
   for(var i = 0;i < 10; i++){
       // TODO
   }
   
   // 突然有一天我想弹出i,这个不清楚i是几
   alert(i)
   ```

变量提升,这个我们之前讲这就是JS语言的特性,后来就有人提议,说这种特性不好,我不知道我程序发生了什么事情,



#### 2. ES6 变量定义

为了解决ES5 变量声明的问题,所以在ES6 新增了两个定义变量的关键词

> let,          就相当于之前的var
>
> const      常量，定义好了以后就不能改变了



先看看let

##### 2.1 基本用法

跟使用es5的var一样

```js
let a = 12;
console.log(a);
```



#### 3. 关于ES6 变量

##### 3.1  变量提升的问题

之前讲var的时候有变量提升,那么let, const 是否具有变量提升呢

```js
let a = 12;
function fn(){
    alert(a);  // 这里当然能用了,这不就是的查找吗
}
fn();

// 那么如果我在函数内定义一个let a; 如下
let a = 12;
function fn(){
    alert(a);  // 报错,这个时候报引用错误
    let a = 5;
}
fn();
// Uncaught ReferenceError: a is not defined
// 报错信息是 a 没有并定义
```

<font style="color:pink;">let,const不会进行变量提升,必须先定义在使用</font>

> 所以必须先定义在使用.用来规范大家编程行为



##### let会引发暂时性死区：

在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

```js
var m = 10;
function fun(){
    m = 20;  //报错。函数在预解析阶段会预读所有的语句，发现了let语句，所以就将这个函数变为了一个m的暂时性死区，此时m不允许在let前被赋值。
    let m;

    console.log(m);
}

fun();
```





##### 3.2. 块级

我们都知道 在语句中 {}叫做块,

我们都知道ES5 只有全局和函数，没有块级，这带来很多不合理的场景。

比如:

 我们刚讲过的for循环的循环变量污染全局

```js
var s = 'hello';
for (var i = 0; i < s.length; i++) {  
  console.log(s[i]);
}
console.log(i); // 5
```

还有就是内层变量可能会覆盖外层变量。

```js
var tmp = 123;

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

这也是为什么我们需要块级的原因



但ES6 let const 所定义的变量具有块

```js
if(true){
    var a = 12;
}
alert(a);  // 能用  之前讲的只有全局和函数,什么if for 在全局就当全局用

// 如果此时换成let
if(true){
    let a = 12;
    // 接下来只能在这里使用
}
alert(a);
```

到了es6 里面

```js
{
    // 块里面就具有了,叫块级
}
```



##### 3.3 同一个变量多次定义的问题

之前咱们说过变量至少要var一次吧;但是我们多次var一个变量也没有问题

```js
// ES5
var a = 1;

var a = 5;
alert(a);    // 弹出5

// ES6 的年代
let a = 1;
let a = 5;
alert(a);  // 报错
//Uncaught SyntaxError: Identifier 'a' has already been declared
```





##### 3.4 关于for循环的循环变量问题

我们之前讲for循环的时候,是不是讲循环变量是当前的

```js
for(var i =0 ;i < 10; i++){
    
}
alert(i);   // 这个时候i指定是一个10

```

那么用let定义的for循环呢,会有哪些不同呢

```js
// 如果我们使用let呢
for(let i =0 ;i < 10; i++){
    console.log(i);  // 在循环里面是可以正常使用的
}
alert(i);   // 报错: 报一个引用错误
//Uncaught ReferenceError: i is not defined
// i未定义

// 如果我们在循环体内在let i呢
for(let i =0 ;i < 10; i++){
	let i = "abc";  // 感觉应该会报错,重复定义嘛,其实不是
    // 可以理解()中的i是父级,循环体内i是子级,互不干扰
    console.log(i);  // 在循环里面是可以正常使用的
}
alert(i);   // 报错: 报一个引用错误
//Uncaught ReferenceError: i is not defined
// i未定义
```



##### 3.5 ES6 允许块级的任意嵌套。

也就是扩展父子级的块

```js
{
    let a = 12;
    console.log(a);  //12
}
console.log(a); // Uncaught ReferenceError: d is not defined

// 如果我开心我是不是可以在块中在加一个块啊
{
    let a = 12;
    {
        let b = 10;
        console.log(b);  // 10
    }
    console.log(a);  //12
}
console.log(a);  // 报错

// 这不叫重复定义,重复定义是不能在同一个中重复定义
// 如果你愿意包含多少层都可以.但是没什么意义,无论你包多少层,外边又访问不到,只有里面能访问到
```



#### let 特点:

> 1. 没有预解析,不存在变量提升
>
>    所以必须先定义在使用.用来规范大家编程行为
>
> 2. 不能重复定义变量(在同一个)
>
> 3. for循环里比较特殊,可以将()中的循环变量理解为父级中的变量,循环体重的变量理解为子中的变量,所以他们let同一个变量不会报错



还记得我们讲for循环里面函数的问题吗

```js
var arr = [];
for(var i = 0; i < 10; i++){
    arr[i] = function(){
        console.log(i);   // 这里我们是不是希望,数组第几项函数执行,打印几啊
    }
}
arr[6]();  //10  结果你发现所有的打印都是10

// 有些小伙伴说我代码没有问题,是的,代码真没有什么问题,这是这么语言特性造成的

// 这是不是就是我们之前讲的函数闭包造成的问题啊

// 这个时候我们换成let去定义
var arr = [];
for(let i = 0; i < 10; i++){
    arr[i] = function(){
        console.log(i);   // 你发现这个时候就符合我们的预期了
    }
}
arr[6]();  // 6
```





#### 示例:

```html
<input type="button" value="aaa"/>
<input type="button" value="bbb"/>
<input type="button" value="ccc"/>

<script>
	var aInput = document.querySelectorAll("input");
    
    for(var i = 0 ;i < aInput.lenght; i++){
        aInput[i].onclick = function(){
			console.log(i)
        }
    }
</script>
```



#### 4. const  声明常量

const声明一个只读的常量。

const: 特性和let一样

只是const定义的变量不能修改,因为人家叫常量,所以不能修改.

```js
let a  = 12;
a = 20;
console.log(a);  // 20
```

有些人可能会说.老师修改怎么了



但是实际开发中有些我是不希望它修改的,比如一些配置文件

```js
const root = 12;
console.log(root);  // 12 你发现能正常使用

// 一旦更改
root = 20;  // 直接报出 错误, TypeError 类型错误
//Uncaught TypeError: Assignment to constant variable.
// 你把一个常量转换到变量的错误
```



const还有一个特点平时我们喜欢先定义变量,在赋初值

```js
var a;
a = 10;
console.log(a);   // 10  没什么问题


// 但是const有问题
const a;
a = 10;
console.log(a);  // 报错  语法错误
//Uncaught SyntaxError: Missing initializer in const declaration 
//缺少初始值在常量定义时

```

也就是说cons定义时必须赋值,不能后赋值,后赋值也是修改,不能修改

如果只是定义不赋值也会报错.

```js
const a;
console.log(a);  // 报错  语法错误
//Uncaught SyntaxError: Missing initializer in const declaration
//缺少初始值在常量定义时
```



验证有无提升

```js
const a = 12;
function fn(){
    alert(a);  // 报错,这个时候报引用错误
   	const a = 5;
}
fn();
// Uncaught ReferenceError: a is not defined
// a未定义
```



有人说不能修改真的假的,看个例子

```js
const arr = ["apple","banana"];
//arr = [];
//console.log(arr);  // 报错
// Uncaught TypeError: Assignment to constant variable.

// 哎 我们学过push修改数组
arr.push("wuwei");
console.log(arr);  //["apple", "banana", "wuwei"]
// 发现可以为什么呢
```

因为数组本来就是引用类型,他们是引用关系,

如果真想定义一个动都不能然它动的,那么对象身上有个方法Object.freeze(),冻结的意思

```js
const arr = Object.freeze(["apple","banana"]);
arr.push("orange");
console.log(arr);  // 这个时候报类型错误
// Uncaught TypeError: Cannot add property 2, object is not extensible
// 不能添加属性,对象是不可扩展的
```

这不是const的问题,这是对象的特性问题



##### 总结: const的特点

1. const`一旦声明变量，就必须立即初始化，不能留到以后赋值。
2. 一旦声明，常量的值就不能改变。



还记得之前理解执行函数

IIFE

```js
(function(){
    // TODO
})();

// 因为之前只有函数

// 现在有块了
// 可以使用
{
   // TODO 
}
```



#### 4. 关于顶层对象属性与全局变量

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。

```js
window.a = 1;
a // 1

a = 2;
window.a // 2
```

顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。

**为了解决这个问题，es6引入的`let` 、`const`和`class`声明的全局变量不再属于顶层对象的属性。**

而同时为了向下兼容，var和function声明的变量依然属于全局对象的属性

```js
var a = 1;
window.a // 1

let b = 1;
window.b // undefined
```

