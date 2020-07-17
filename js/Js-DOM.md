## DOM

  ---2020/04/14

------

## 1.symbol     和	json

#### 			symbol:

> ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法，新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入`Symbol`的原因。

ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：**undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）**。

​			<font style="color:skyblue;">Symbol 值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。 </font>

```js
let s = Symbol();
typeof s
// "symbol"
```

上面代码中，变量s就是一个独一无二的值。typeof运算符的结果，表明变量s是 Symbol 数据类型，而不是字符串之类的其他类型。

​	<font style="color:tomato;font-size:20px">注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。 </font>

Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```js
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

上面代码中，`s1`和`s2`是两个 Symbol 值。如果不加参数，它们在控制台的输出都是`Symbol()`，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。

如果 Symbol 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 Symbol 值。

```js
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```

⚠️注意，Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。

#### 		**json:**

### 1.JSON 格式

`JSON` 格式（`JavaScript Object Notation` 的缩写）是一种用于数据交换的文本格式。

- 相比 

  ```
  XML
  ```

   格式，

  ```
  JSON
  ```

   格式有两个显著的优点：

  - 书写简单，一目了然；
  - 符合 JavaScript原生语法，可以由解释引擎直接处理，不用另外添加解析代码。

每个 JSON 对象就是一个值，可能是一个数组或对象，也可能是一个原始类型的值。总之，只能是一个值，不能是两个或更多的值。

- JSON 对值的类型和格式有严格的规定。
  - 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
  - 原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
  - 字符串必须使用双引号表示，不能使用单引号。
  - 对象的键名必须放在双引号里面。
  - 数组或对象最后一个成员的后面，不能加逗号。

以下都是合法的 JSON：



```json
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```

以下都是不合法的 JSON：



```jsx
{ name: "张三", 'age': 32 }  // 属性名必须使用双引号

[32, 64, 128, 0xFFF] // 不能使用十六进制值

{ "name": "张三", "age": undefined } // 不能使用 undefined

{ "name": "张三",
  "birthday": new Date('Fri, 26 Aug 2011 07:13:10 GMT'),
  "getName": function () {
      return this.name;
  }
} // 属性值不能使用函数和日期对象
```

- 注意，null、空数组和空对象都是合法的 JSON 值。

### 2.JSON 对象

JSON对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：JSON.stringify()和JSON.parse()。



### **3.JSON.stringify()**

#### 				3.1基本用法

> JSON.stringify方法用于将一个值转为 JSON 字符串。
>       该字符串符合 JSON 格式，并且可以被JSON.parse方法还原。

- 下面代码将各种类型的值，转成 JSON 字符串。



```jsx
JSON.stringify('abc') // ""abc""
JSON.stringify(1) // "1"
JSON.stringify(false) // "false"
JSON.stringify([]) // "[]"
JSON.stringify({}) // "{}"

JSON.stringify([1, "false", false])
// '[1,"false",false]'

JSON.stringify({ name: "张三" })
// '{"name":"张三"}'
```

- 注意，对于原始类型的字符串，转换结果会带双引号。



```jsx
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```

上面代码中，字符串foo，被转成了""foo""。这是因为将来还原的时候，内层双引号可以让 JavaScript 引擎知道，这是一个字符串，而不是其他类型的值。



```jsx
JSON.stringify(false) // "false"
JSON.stringify('false') // "\"false\""
```

上面代码中，如果不是内层的双引号，将来还原的时候，引擎就无法知道原始值是布尔值还是字符串。

- 如果对象的属性是undefined、函数或 XML 对象，该属性会被JSON.stringify过滤。



```jsx
var obj = {
  a: undefined,
  b: function () {}
};

JSON.stringify(obj) // "{}"
```

上面代码中，对象obj的a属性是undefined，而b属性是一个函数，结果都被JSON.stringify过滤。

- 如果数组的成员是undefined、函数或 XML 对象，则这些值被转成null。



```jsx
var arr = [undefined, function () {}];
JSON.stringify(arr) // "[null,null]"
```

上面代码中，数组arr的成员是undefined和函数，它们都被转成了null。

- 正则对象会被转成空对象。



```jsx
JSON.stringify(/foo/) // "{}"
```

- JSON.stringify方法会忽略对象的不可遍历的属性。



```csharp
var obj = {};
Object.defineProperties(obj, {
  'foo': {
    value: 1,
    enumerable: true
  },
  'bar': {
    value: 2,
    enumerable: false
  }
});

JSON.stringify(obj); // "{"foo":1}"
```

上面代码中，bar是obj对象的不可遍历属性，JSON.stringify方法会忽略这个属性。

#### 3.2第二个参数

JSON.stringify方法还可以接受一个数组，作为第二个参数，指定需要转成字符串的属性。

- 下面代码中，JSON.stringify方法的第二个参数指定，只转prop1和prop2两个属性。



```bash
var obj = {
  'prop1': 'value1',
  'prop2': 'value2',
  'prop3': 'value3'
};

var selectedProperties = ['prop1', 'prop2'];

JSON.stringify(obj, selectedProperties)
// "{"prop1":"value1","prop2":"value2"}"
```

- 这个类似白名单的数组，只对对象的属性有效，对数组无效。



```bash
JSON.stringify(['a', 'b'], ['0'])
// "["a","b"]"

JSON.stringify({0: 'a', 1: 'b'}, ['0'])
// "{"0":"a"}"
```

上面代码中，第二个参数指定 JSON 格式只转0号属性，实际上对数组是无效的，只对对象有效。

- 第二个参数还可以是一个函数，用来更改JSON.stringify的返回值。



```csharp
function f(key, value) {
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}

JSON.stringify({ a: 1, b: 2 }, f)
// '{"a": 2,"b": 4}'
```

上面代码中的f函数，接受两个参数，分别是被转换的对象的键名和键值。如果键值是数值，就将它乘以2，否则就原样返回。

- 注意，这个处理函数是递归处理所有的键。



```jsx
var o = {a: {b: 1}};

function f(key, value) {
  console.log("["+ key +"]:" + value);
  return value;
}

JSON.stringify(o, f)
// []:[object Object]
// [a]:[object Object]
// [b]:1
// '{"a":{"b":1}}'
```

上面代码中，对象o一共会被f函数处理三次，最后那行是JSON.stringify的输出。第一次键名为空，键值是整个对象o；第二次键名为a，键值是{b: 1}；第三次键名为b，键值为1。

- 递归处理中，每一次处理的对象，都是前一次返回的值。



```csharp
var o = {a: 1};

function f(key, value) {
  if (typeof value === 'object') {
    return {b: 2};
  }
  return value * 2;
}

JSON.stringify(o, f)
// "{"b": 4}"
```

上面代码中，f函数修改了对象o，接着JSON.stringify方法就递归处理修改后的对象o。

- 如果处理函数返回undefined或没有返回值，则该属性会被忽略。



```jsx
function f(key, value) {
  if (typeof(value) === "string") {
    return undefined;
  }
  return value;
}

JSON.stringify({ a: "abc", b: 123 }, f)
// '{"b": 123}'
```

上面代码中，a属性经过处理后，返回undefined，于是该属性被忽略了。

#### 3.3第三个参数

JSON.stringify还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过10个）；如果是字符串（不超过10个字符），则该字符串会添加在每行前面。



```bash
JSON.stringify({ p1: 1, p2: 2 }, null, 2);
/*
"{
  "p1": 1,
  "p2": 2
}"
*/

JSON.stringify({ p1:1, p2:2 }, null, '|-');
/*
"{
|-"p1": 1,
|-"p2": 2
}"
*/
```

#### 3.4参数对象的 toJSON 方法

如果参数对象有自定义的toJSON方法，那么JSON.stringify会使用这个方法的返回值作为参数，而忽略原对象的其他属性。

- 下面是一个普通的对象。



```kotlin
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  }
};

JSON.stringify(user)
// "{"firstName":"三","lastName":"张","fullName":"张三"}"
```

- 现在，为这个对象加上toJSON方法。



```kotlin
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  },

  toJSON: function () {
    return {
      name: this.lastName + this.firstName
    };
  }
};

JSON.stringify(user)
// "{"name":"张三"}"
```

上面代码中，JSON.stringify发现参数对象有toJSON方法，就直接使用这个方法的返回值作为参数，而忽略原对象的其他参数。

- Date对象就有一个自己的toJSON方法。



```jsx
var date = new Date('2015-01-01');
date.toJSON() // "2015-01-01T00:00:00.000Z"
JSON.stringify(date) // ""2015-01-01T00:00:00.000Z""
```

上面代码中，JSON.stringify发现处理的是Date对象实例，就会调用这个实例对象的toJSON方法，将该方法的返回值作为参数。

- toJSON方法的一个应用是，将正则对象自动转为字符串。因为JSON.stringify默认不能转换正则对象，但是设置了toJSON方法以后，就可以转换正则对象了。



```jsx
var obj = {
  reg: /foo/
};

// 不设置 toJSON 方法时
JSON.stringify(obj) // "{"reg":{}}"

// 设置 toJSON 方法时
RegExp.prototype.toJSON = RegExp.prototype.toString;
JSON.stringify(/foo/) // ""/foo/""
```

上面代码在正则对象的原型上面部署了toJSON方法，将其指向toString方法，因此遇到转换成JSON时，正则对象就先调用toJSON方法转为字符串，然后再被JSON.stingify方法处理。

#### 4.JSON.parse()

- JSON.parse方法用于将 JSON 字符串转换成对应的值。



```jsx
JSON.parse('{}') // {}
JSON.parse('true') // true
JSON.parse('"foo"') // "foo"
JSON.parse('[1, 5, "false"]') // [1, 5, "false"]
JSON.parse('null') // null

var o = JSON.parse('{"name": "张三"}');
o.name // 张三
```

- 如果传入的字符串不是有效的 JSON 格式，JSON.parse方法将报错。



```jsx
JSON.parse("'String'") // illegal single quotes
// SyntaxError: Unexpected token ILLEGAL
```

上面代码中，双引号字符串中是一个单引号字符串，因为单引号字符串不符合 JSON 格式，所以报错。

- 为了处理解析错误，可以将JSON.parse方法放在try...catch代码块中。



```jsx
try {
  JSON.parse("'String'");
} catch(e) {
  console.log('parsing error');
}
```

- JSON.parse方法可以接受一个处理函数，作为第二个参数，用法与JSON.stringify方法类似。



```csharp
function f(key, value) {
  if (key === 'a') {
    return value + 10;
  }
  return value;
}

JSON.parse('{"a": 1, "b": 2}', f)
// {a: 11, b: 2}
```

上面代码中，JSON.parse的第二个参数是一个函数，如果键名是a，该函数会将键值加上10。

------

## 2.DOM操作 html 

> DOM : 操作网页的一个接口 --文档对象模型
>          								DOM: 最小组成单位 : 叫做节点  node
>          								文档的树形结构--DOM树

​        **节点树:**
​        *       元素节点  属性节点   文本节点
​                *       注释节点

```js
 <div id="wrap">
        天空
        <p>很蓝</p>
        我们都能
    </div>
    <script>
        let oWrap = document.getElementById("wrap");
        console.dir(oWrap);
        console.log(oWrap.childNodes); // childNodes 子节点
        console.log(oWrap.childNodes[0].nodeType); // 3 节点类型
        console.log(oWrap.childNodes[0].nodeName); // #text  节点名字
        console.log(oWrap.childNodes[0].nodeValue); // 节点内容  换行空格空格换行

        console.log(oWrap.childNodes[1].nodeValue); // null

        /*
        *   元素节点的  nodeValue 是没有意义的
        *
        *   用处意义 ：
        * */
        let p = document.querySelector("p");
        p.onclick =function(){
            console.log("正常获取p标签");
        };
        setTimeout(()=>{
            oWrap.innerText += "看见云朵"
            //  oWrap.childNodes[2].nodeValue = "真美";
             // 注意的地方
        },30)
 </script>
```

#### 2.1子元素节点

```js
 <div id="wrap">
        <p>00001</p>
        <p>00002</p>
        <p>00003</p>
    </div>
    <script>
        //
        let oWrap = document.getElementById("wrap");
        console.log(oWrap.childNodes); // 7

        // 只想获取元素节点
        // 获取一个元素的所有子元素节点
        console.log(oWrap.children);
        console.log(oWrap.children[1]);

        // NodeList 也是一个类数组,可以forEach
        // HTMLCollection(类数组) 不可以forEach  ,  ...解开

        let html = [...oWrap.children];
        html.forEach(()=>{
            console.log(1111)
        })
 </script>
```

![image-20200414233901108](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200414233901108.png)

#### 2.2父节点

```js
<div class="box">
        <div id="wrap">
            <p id="yanxin">言心</p>
        </div>
    </div>

    <script>
        let yan = document.getElementById("yanxin");

        console.log(yan.parentNode); // 父节点

        // 定位父级, 参照谁定位

        console.log(yan.offsetParent);

        //  固定定位 是没有参照显示区域的,比较特殊,
        // 比较特殊  --- 没有什么东西能够表示窗口 -- null
    </script>
```

##### **2.2.0不常用**

```js
<div id="wrap">
        <p>000001</p>
        <p>000002</p>
        <p id="ss">000003</p>
        <p>000004</p>
        <p>000005</p>
    </div>
    <script>
        let wrap = document.getElementById("wrap");

        // 第一个子节点  firstChild   第一个子节点
        console.log(wrap.firstChild);
        //  lastChild

        //  第一个子元素节点
        console.log(wrap.firstElementChild);
        // lastElementChild

        // 获取第一个子元素
        console.log(wrap.children[0]);
        // 获取最后一个
        console.log(wrap.children[wrap.children.length-1]);


        /*
        *   下一个兄弟:
        *
        *   上一个哥哥:
        *
        * */

        let ss = document.getElementById("ss");

        // 下一个紧挨着的兄弟
        console.log(ss.nextSibling);
        console.log(ss.nextElementSibling);

        // 哥哥
        console.log(ss.previousSibling);
        console.log(ss.previousElementSibling)

        // 不兼容  ie8  判断  标志性   : window.getComputedStyle

        // 兼容写发
        function getNextSibling(node){
            if (window.getComputedStyle){
                return node.nextElementSibling
            }else{
                return node.nextSibling
            }
        }
        getNextSibling(ss);
    </script>
```

#### 2.3节点删除

```js
 <div id="wrap">
        <p>000001</p>
        <p>000002</p>
        <p>000003</p>
    </div>
    <script>
        let wrap = document.getElementById("wrap");

        // 增加  删除   重点

        let child = wrap.children;
        let p1 = child[0];
        let p2 = child[1];
        let p3 = child[2];

        p1.onclick = function(){
            console.log(this.innerHTML)
        };
        p2.onclick = function(){
            console.log(this.innerHTML)
        };
        p3.onclick = function(){
            console.log(this.innerHTML)
        };

        // 原生js ,不能自己删除自己  通过父节点删除 ---> 爸爸kill儿子

        wrap.removeChild(p2); // 删除节点

        // 问题? p1  p3  还是原来的自己
        console.log(p1);
        console.log(p3);

        // 原因:无论外界发生什么变化, 只要是一个变量存储某一个节点,这个节点他就是这个节点

        // 谁变化?  wrap.children 是动态的
        console.log(child);

        // 总结: children 获取的子元素节点集合是动态的

        console.log(p2);
        // 存在内存里边

```

#### 2.4节点增加

```js
 <div id="wrap">
        <p>000001</p>
    </div>
    <script>
        let wrap = document.getElementById("wrap");

        // 要有元素节点
        // 1. 创建节点

        let p2 = document.createElement("p");  // 创建节点

        // 先搞好所有内容,然后添加到页面,少一次渲染

        p2.classList.add("yanxin");
        p2.innerText = "小言心";

        // 父级追加子节点
        wrap.appendChild(p2);

        let pp = document.querySelector(".yanxin");
        pp.onclick = function(){
            console.log(this.innerHTML);
        };


        // 添加文本
        let txt = document.createTextNode("hahahahhahahhah ");

        wrap.appendChild(txt);

```

#### 2.5 创建多个节点

```js
<ul id="box">
        <li>0000000</li>
    </ul>
    <script>
        // 创建多个节点
        let ul = document.getElementById("box");

        //  循环添加  10 -- li
        // for (let i = 0; i <10 ; i++) {
        //     ul.innerHTML += `<li>我是第${i}标签</li>`
        // }

        // for (let i = 0; i <100000; i++) {
        //    let li = document.createElement("li");
        //    li.innerHTML = i +"   我是谁啊";
        //    ul.appendChild(li);
        // }

        // 以上两种  频繁的渲染页面,浪费性能,增添加负担
-----------------------------------------------------------
        // 文档碎片  /  片段  : createDocumentFragment()

        let fra = document.createDocumentFragment();

        for (let i = 0; i < 10 ; i++) {
            let li = document.createElement("li");
            li.innerHTML = i +"   个文本";
            fra.appendChild(li);
        }
        // 最后添加到页面
        ul.appendChild(fra);

```

#### 2.6中间添加节点

```js
 <div id="wrap">
        <p>000000</p>
        <p>111111</p>
        <p>222222</p>
    </div>
    <script>
        let ow= document.getElementById("wrap");
        let p1 = document.querySelector("#wrap>p:nth-child(1)");
        let p3 = document.querySelector("#wrap>p:nth-child(3)");

        let div = document.createElement("div");
        div.innerText = "我的妈呀";
        div.style.color="red";

        // 在已有的基础上前边添加一个节点 :insertBefore(新节点,老节点)
        // ow.insertBefore(div,p1);
        // ow.insertBefore(div,p3);
    </script>
```

#### 2.7替换节点

```js
  <script>
        //replaceChild  用新节点替换某个子节点，
        // 第一个参数为新节点，第二个参数为已存在的某个子节点。
        // ow.replaceChild(div,p3)

        // 创建文本节点:
        let txt = document.createTextNode("hello新添加");

        // 添加到wrap里边
        ow.appendChild(txt);
  </script>
```

## 3.BOM

> 什么是BOM？
> Broswer Object Model
> BOM和DOM类似也是一个编程接口,这个编程接口让JavaScript有能力与浏览器对话 
> 和DOM不同的是,**DOM的核心是document,而BOM的核心是window。** 在全局环境中的变量&&函数声明自动成为window的属性和值

```js
1. location =>浏览器地址栏信息
location.href 地址栏中完整的url
location.protocol 地址栏的协议
location.hostname 地址栏的主机名
location.port 地址栏的端口号
location.host 地址栏的主机名+端口号
location.pathname 地址栏的路径
location.search 地址栏?后面的字符串
location.hash 地址栏#后面的字符串
location.reload  刷新页面

2.history =>某窗口的历史页面
history.length 历史页面个数 history.back() 跳转到前一个页面 history.forward() 跳转到后一个页面
history.go(参数) 跳转到第几个页面 参数为数字 可正可负
.go()
                        正值-->前进打开页面
                        负值-->后退打开页面
                        0 ---->会刷新页面
3. navigator =>浏览器的信息
navigator.userAgent 浏览器的版本号
location  对象
                    .href ---
                    
             history  历史对象
                   
            navigator--当前浏览器的信息
                   userAgent
```

😂举个栗子：

```js
<script> 	
	    console.log(location);
        console.log(window.history);
        console.log(window.navigator);

  //close   --- 关闭当前标签页
        document.onclick = function(){
            window.close();
        }
</script> 	
```

