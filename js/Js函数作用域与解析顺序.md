## 	<font style="color:yellowgreen;">作用域与解析顺序</font>

------2020.03.28

------

####  作用域: 

####               定比变量之后,变量能够在一定的范围起作用  起作用的范围 就叫做  作用域

​					 <font style="color:skyblue;">script标签是最大的作用域,定义在这个的变量 成为全局变量</font>  

#### JS: GO 和 AO 

GO： global object 即 全局上下文
		AO :activation object 活跃对象，函数上下文,在函数执行之前进行的一个步骤

[🌏参阅博客]: https://www.cnblogs.com/ycherry/p/11638041.html	"AO和GO"

```js
			 声明 :  var   function  确定范围--作用域的范围

                在任何地方都可以访问全局作用域下的变量
                在 es5  -- 函数 产生一个局部作用域

                作用域链:  
                    使用某个变量,会从当前作用域查找,有就是用,没有就向上一层作用域查找,
                    以此类推,直到全局作用域还没有找到,就会报错 .
function outer(){
  var a = 3; //a的作用域就是outer
  function inner(){
    var b = 5; //b的作用域就是inner
    console.log(a); //能够正常输出3，a在本层没有定义，就是找上层
    console.log(b);  //能够正常输出5
 }
  inner();
}
outer();
console.log(a); //报错，因为a的作用域outer
```

#### 作用域链

当遇见一个变量时，JS引擎会从其所在的作用域依次向外层查找，查找会在找到第一个匹配的标识符的时候停止.

```js
function outer(){
  var a = 3; //a的作用域就是outer
  function inner(){
    var b = 5; //b的作用域就是inner
    console.log(a); //能够正常输出3，a在本层没有定义，就是找上层
    console.log(b);  //能够正常输出5
 }
  inner();
}
outer();
console.log(a); //报错，因为a的作用域outer
```



### 多层嵌套，如果有同名的变量，那么就会发生“遮蔽效应”：

```js

var a = 2; //全局变量
function fn(){
  var a = 3;     //就把外层的a给遮蔽了，这函数内部看不见外层的a了。
  console.log(a); //输出3，变量在当前作用域寻找，找到了a的定义值为5
}
fn();
console.log(a);  //输出2,变量在当前作用域寻找，找到了a的定义值为1
```

------

#### Js 执行流程:

```js
                1. 预解析
                    找变量,检查语法是否符合规范
                    var  function
                2. 执行
                    运行js 代码 ,赋值等操作
---------------------------------------------------------------------------
 alert(a);  // undefined
  var a;

```
------

####  解析顺序:

1. 定义 (预解析): ---  找 var / function 定义出的所有变量
                        找  var  
                        找 function

2. 执行 --- 从上到下执行代码
          赋值 等 执行操作

      > ```js
      > 例1：
      > var c = 10;
      >  alert(c) // 10
      > 
      >             /*
      >                 GO 
      >                 1. 预解析
      >                     var c
      >                 2. 执行
      >                     c = 10
      >                     alert(c)  // 10
      > 
      >             */
      > ```
```js
      

      例2：
      		var x = 5;
                  a();
                  function a(){
                      alert(x);  // 
                      var x = 10;
                  }
                  alert(x);  //  
                  /*
                      GO: 
                          1. 预解析
                              找 var function
      
                              var x;
                              function a() { ... }  // 没有执行就是一坨代码
                          
                          2. 执行
                              x = 5;
                              a() ===> 执行 遇到作用域 AO =
                                                      1. 预解析
                                                          var x
                                                      2. 执行
                                                          alert(x) // undefined
                                                          x = 10
                              
                              alert(x) //  5
                  
                  */
```

####  预解析 的几种情况

> ```js
> 				 1. var 和 var 重名
>                     	只看一个,就近原则
>                    2. var 和 function 重名
>                               function 优先
>                     3. function 和  function 重名
>                               后边覆盖前边
> ```

```js
 例3:					
  					var a = 10 ;
                        var a = 20;
                        alert(a);//20
  ---------------------------------------------------
   			function b(){
                  console.log(1);
              }
              function b(){
                  console.log(2);
              }
              b();//只看最好一个
  -------------------------------------------------------
  			//var  c = 10 ;
              function c(){
                  console.log(3);
              }
              var c ;
  
  🔺		alert(c);  //  没有 var c = 10; 弹出的是函数体
  		alert(c);  //  没有 var c ; 有 var c = 10; 弹出的是 10
           alert(c);  // var c 在 下边    有 var c = 10; 弹出的是 10
       	 alert(c); // var  c = 10 在  下边    弹出的是 10
  
 
   例4：   				
  					alert(a);
                         var a = 10;
                         alert(a);
                         function a(){alert(20)}
                         alert(a);
                         var a = 30;
                         alert(a);
                         function a(){alert(40)}
                         alert(a);
  
  			GO:
                  1. 预解析
                      function a(){alert(40)}
                  2. 执行:
                      alert(a)  //  40 的函数体
                      a = 10 ;
                      alert(a) // 10 
                      alert(a)  // 10 
                      a = 30
                      alert(a) // 30 
                      alert(a)  // 30
```


​      
```js

  例5：		  
             var a = 10;
              alert(a);
              a();
              function a(){
                  alert(20);
              }
              
                  //GO:
                      1.  预解析
                          function a(){ ... }   
                      2.  执行
                          a = 10;
                          alert(a)  // 10 
                          a()  报错:
             // 报错的分析
                  a = 10
                  a() 
                  10()

```


​     


​      