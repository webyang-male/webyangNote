## ES6解构赋值

​		--2020/04/09

------

**回顾**

> ```js
> let --- 声明的变量不能提前使用, 不能重复定义同一个变量啊(let var const function )
> let --- 块级作用域   {这个里边用let 定义的变量}  但是  自定义对象不是
> const   --- 声明常量,并且在一开始就要赋值,否则报错.不能后期再赋值,否则报错. 定义的常量的值不能修改
> ```

### **顶层对象**

 		<font style="color:skyblue;">es5 顶层对象是 **window** </font>

```js
 var a = 10;
 console.log(window.a)
```

​		<font style="color:skyblue;">es6 顶层对象  : global</font> 
​       		🍀 优点 : 不再会给window 添加属性了

```js
 let b = 20;
 console.log(window.b)  // undefined
```

------

### 解构赋值

<font style="color:gold;">按照一定模式 从数组和对象中提取值,然后在对变量进行赋值 (按照位置一一对应赋值)</font> 

#### 			1.数组的结构赋值

```js
	    // let a = 1;
        // let b = 2;
        // let c = 3;

        let [a,b,c] = [1,2,3]
        // 一一对应赋值
---------------------------------------------------------
	    let [,,x] = ["雷哥","小妹","人食"]
        console.log(x);//人食
----------------------------------------------------------
	    let [aa,bb,cc] = [11,22]
        console.log(aa,bb,cc);  // 11 22 undefined

```

<font style="color:yellowgreen;">解构赋值允许指定默认值</font> 

```js
 		let [a = true] = [];
        console.log(a);//true

        let [x,y = "cc"] = ["aa"];
        console.log(x,y);  // aa cc

        // let [w,e = "cc"] = ["aa",undefined];
        // console.log(w,e);  // aa cc

        let [w,e = "cc"] = ["aa",null];
        console.log(w,e);  // aa null
```

💗<font style="color:tomato;"> 只有当一个数组成员  严格等于   undefined,默认值才会生效</font> 

```js
 		let [zz = 1] = [null]
        console.log(zz);  // null


        let [s = 1,d = s] = [];
        console.log( s,d);  //  1   1

        let [ss = 1,dd = ss] = [10];
        console.log( ss,dd);  //  10  10

        let [sss = 1,ddd = sss] = [10,20];
        console.log( sss,ddd);  //   10   20

        let [ff = gg,gg = 1 ] = [];
        console.log( ff);  // 报错
        console.log(gg);
        // 因为 ff用gg做默认值时,gg 还没有声明
```

<font style="color:red;font-size:20px">⚠️默认值可以引用解构赋值的其他变量,  但是该变量必须  **已经声明**!</font> 

------

#### 2.对象的结构赋值

```js
		let {a,b} = { a:1,b:2 };
        console.log(a,b); // 1 2

        let {aa,bb} = { c:1,d:2 };
        console.log(aa,bb); // undefined  undefined

```

> 对象是没有次序,  变量名必须和属性名同名,才能够取到值
>        等号右边的对象没有 aa ,bb  这个属性,所以变量aa,bb取不到值,所以等于undefined

😅**变量名和属性名不一致?**

```js
  let {  cc : vv } = { cc :123456,bb:123 };
        console.log(vv);// 123456

```

> 先找到 同名属性, 然后在赋值给对应的变量.
>           所以真正被赋值的是    后者,而不是前者

```js
 		let obj = { one : 1,two:2 };
        let { one : h , two : g} = obj;
        console.log(h,g);   // 1   2
```

🌸我们再看一个案例：

```js
	    let {  foo : baz  } = {  foo : 123 , bar : 456  };
        console.log(baz);//123
        console.log(foo);  // 报错  未定义
```

 foo 是匹配模式, **baz才是变量**,真正被赋值的是baz ,  而不是模式foo

<font style="color:red;font-size:20px">⚠️默认值  生效  的条件: ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值。对象的属性值  严格等于 undefined	!</font> 

```js

        let { x :ab } = {};
        console.log(ab);  // 3

        let { xx , y = 85 } = { xx :12};
        console.log(xx); // 12
        console.log(y);  // 85

        let { a : yy = 3} = { };
        console.log(yy); // 3

        let { bb : cc = 3} = { bb : 56};
        console.log(cc); // 56
----------------------------------------------------------
 let { vv = 3 } = { vv : null };
        console.log(vv);//null

```

------

#### 3.函数的解构赋值

**es5**

```js
 function fn( [x,y] ){
            console.log(x,y);
            return x+y;
        };
        console.log(fn( [2,3]));
2 3
5
```

**ES6**

例1：

```JS
 function a( {x=0,y=0} = {} ){
            return [x,y];
        }
        
        console.log(
            a( { x:3,y:10 } )
        );
        
        console.log(
            a( { x:3} )
        );
        
        console.log(
            a( { } )
        );
        
        console.log(
            a()
        );
 (2) [3, 10]
 (2) [3, 0]
 (2) [0, 0]
 (2) [0, 0]
```

例2：

```JS
 function a( {x:x=0,y:y=0} = {x:20} ){
            return [x,y];
        }

        console.log(
            a( { x:3,y:10 } )
        );

        console.log(
            a( { x:3} )
        );

        console.log(
            a( { } )
        );

        console.log(
           a()
        );
(2) [3, 10]
(2) [3, 0]
(2) [0, 0]
(2) [20, 0]
```

------

##### 函数返回多个值结合解构赋值

 函数返回多个值------放在数组 或者 对象里返回

```js
   function a() {
            return ["小a", "小b", "小c"]
        }
        let [x, y, z] = a();
        console.log(x, y, z);//小a 小b 小c
-------------------------------------------------------------
        function fn() {
            return {
                name: "yanxin",
                age: 16,
                job: "讲师"
            }
        }

        let { name, age, job } = fn();
        console.log(
            name,
            age,
            job
        )//yanxin 16 讲师

		//也可以这样写
        let { name: na, age: ag, job: jb } = fn();
        console.log(
            na, ag, jb
        )//yanxin 16 讲师
```

