## ES6

**拓展运算符、箭头函数**

​			-----2020/04/11

------

### 一. 扩展运算符

扩展运算符（spread）是三个点（ ... ）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
	rest参数  形式为 : ...变量名 .用于获取多余的参数

```js
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5
[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]
```

该运算符将一个数组，变为参数序列。

```js
 		 let a = [1,2,3];
          let b = a;  // 引用关系 ,两边都要变化
          b.push(56)
          console.log(a);
```

**1.新增数据对比：**

```js
		// es5
 		   var  a = [1,2,3];
            var b= [];
            for (var i = 0;i<a.length;i++){
                b[i] = a[i]
            }
            b.push(23);
            console.log(a)

            //es6
            let b = [...a];
            b.push(14);
            console.log(b)
```

⚠️... 后边的数据是有要求的 : 必须有**iterator接口**的,才能过用

> iterator接口 : ?
>             *   接口机制,处理不同的数据结构的 --- 他是一种接口
>                         *   为了各种不同的数据结构提供统一的访问机制
>                                     *   任何数据只要部署了它,就可以进行遍历操作
>                                                 *   哪些数据拥有iterator接口:
>                                                             *           **array , set ,map,string,TypeArray,函数的arguments ,NodeList对象**

```js
 			let str = "世界真大,你可真可爱";
            console.log(...str);//空格输出---世 界 真 大 , 你 可 真 可 爱

            // 求最大值 小
            let c = [1,2,3,45,65,78,100];

            console.log( Math.max(...c));//100

            //  apply

            let max = Math.max.call(Math,...c)
            console.log(max)//100

```

**2.与解构赋值结合使用**

#### 数组

```js
 	    let a = [1,2,3];
        let b = [4,5,6];
        
        let c = [...a,...b];
        console.log(c)

//(6) [1, 2, 3, 4, 5, 6]
```

再看一个栗子：

```js
  let [a,b] = [1,2,3,4,5,6,7,8,9,10]
       console.log(a,b)//1,2
```

这个时候不知道数组的后边还有多少个东西,希望用数组接收后边所有的东西

```js
 		let [a,b,...c]= [1,2,3,4,5,6,7,8,9,10]
        console.log(a,b)
        console.log(c) // 数组,接收剩下来的所有数据

 🔺  注意: 中间不能放,只能放在最后
        let [a,...c,b]= [1,2,3,4,5,6,7,8,9,10];
        console.log(a,b);
        console.log(c); // 报错 Uncaught SyntaxError: Rest element must be last element
```

**空数组**

```js
	    let [ x,y,...z ] = [1,2];
        console.log(x);
        console.log(y);
        console.log(z);
1
2
[]
```

#### 函数

```js
function n(q,w,...e){
            console.log(q);
            console.log(w);
            console.log(e);
        }
        n(1,2,3,4,5,6,78,9);
1
2
(6) [3, 4, 5, 6, 78, 9]

 //  arguments  用...代替
        function n(...g){
            console.log(g);
        }
n(1,2,3,4,5,6,78,9);//(8) [1, 2, 3, 4, 5, 6, 78, 9]

```

```js
<body>
    <p>0001</p>
    <p>0002</p>
    <p>0003</p>
    <script>
        let vv = [...document.getElementsByTagName("p")]
        console.log(vv);//(3) [p, p, p]
    </script>
</body>
```

------

### 二、 箭头函数   	=>

```js
 // function a(){
        
        // }

        // 匿名函数才合适 改变成箭头函数

        let a = function (){
            console.log(11111)
        };
        a();//11111

        let b = (c)=>{
            console.log(c)
        };
        b(11111);//11111
```

> 有且仅有一个形参,可以省略<font style="color:pink;">*圆括号*</font>
> 	   返回值是一条语句,可以是省略<font style="color:yellowgreen;">*大括号和return*</font>

```js
let s = v => console.log(v);
        s(22);//22
```

返回值是一个对象,那就在外边必须加上圆括号

```js
		let  obj = ()=>({name:"学习"});
        console.log( obj())
 // 相当于
        let s = function (v){
            return 3
        }
        s();
```

```js
 	    //   let hh = function (x,y){
        //     return x+y;
        // }
        // console.log(  hh(3,4))//7

        let hh = (x,y)=>x+y;
        console.log(  hh(3,4) )//7
```

#####  <font style="color:tomato;">注意: 1. 没有自己的this, 在哪里定义,this 就在哪里 ,不适合事件函数 </font>

```js
  let hh = () => {
            console.log(this)
        };
        // hh();
        document.onclick = hh;

//写法2
        document.onclick = function () {
            let mm = () => {
                console.log(this);
            }
            mm();
        }

```

##### <font style="color:tomato;">2.没有arguments  ， 用rest参数代替</font>

```js
 let fn = (...r)=>{
            console.log(
                r
            );  
        }
        fn(1,2,3);//123
```

##### 三、遍历循环

```js
 let arr = ["yanxin", "童童", "whoami "];
        // keys(),valuse(),entries()
        console.log([...arr.keys()]); // 数组的下标 :  键
        console.log([...arr.values()]); // 数组项
        console.log([...arr.entries()]) // 键值对, 下标和数据

       // for  ...of  -- 一般遍历对象的

        // for (let key of arr.entries()){
        //     console.log(key);
        // }

        // 结构赋值
        for (let [key,value] of arr.entries()){
           console.log(key,value);
         }
```

![image-20200412190639606](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200412190639606.png)

```js
输出结果如上
// 在 es5  for ... in ,遍历对象存在问题
         var obj = {
            name:"zzy",
            age:18,
            job:"学生",
            sex:2
        };
       
         console.log(obj);
        
        for (var key in obj){
            console.log(key);
            console.log(obj[key])
        };
---------------------------------------------------------------
    
     // es6  for ...of
        for (let key of obj){
            console.log(key);  // 报错: obj 不是iterable的接口
            console.log(obj[key])
        }

        // Object 所有对象的父类
        for (let [key,value] of Object.entries(obj)){
            console.log(key,value);
        }

```

