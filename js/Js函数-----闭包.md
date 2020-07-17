## <font style="color:Lavender;">闭包</font>

----2020.03.29

------

### RETURN例题回顾

```js
					   fn()();
			            var a = 0;
			            function fn(){
			                alert(a);  // 
			                var a = 3;
			                function c(){
			                    alert(a)   //
			                }
			                return c;
			            }
  /*
			                GO:
			                fn() --- AO
			                        预解析:
			                            var a
			                            function c(){ ... }
			                        执行:
			                           alert(a) ---- undefined
			                           a = 3
			                c()---AO
			                        执行:
			                            alert(a)  -- 3 
			                a = 0;
			            
			            */
-----------------------------------------------------------------------------------------
					 var a = 5;
			            function fn(){
			                var a = 10;
			                alert(a);
			                function b(){
			                    a ++;
			                    alert(a);
			                }
			                return b;
			            }
			            var c = fn();
			            c();
			            fn()();
			            c();
			            /*
			                c = fn()  -- fn执行--- alert(a)   -- > 10
			
			                c()---  alert(a)  -- > 11 
			
			                fn()()
			                第一个()执行  --  alert(a) -->  10 
			                第二个() 执行 --  alert(a)  -- > 11
			                
			                c() -- a++ (这里a ) alert(a)  --- >  12
			            
			                结果: 10 11 10 11 12
```

------

🌼<font style="color:Moccasin;font-size:20px">垃圾回收机制:</font>

```js
		js 自动 回收不用的变量
                    1. 全局变量不会回收,除非当前页面关闭
                    2. 局部变量 在使用完之后,就会回收
                    3. 构成闭包.持续使用变量,就不会被回收
```
```js
 			var a = 10;
            console.log(a);
            
            (function(){
                var a = document.getElementById("wrap");
                a.innerHTML = 12345;
                a.style.color = "red";
            })();
```

------

## <font style="color:red;">闭包:</font>

一个函数可以把它自己内部的语句，和自己声明时所处的作用域一起封装成了一个密闭环境，我们称为“闭包”
（Closures）。
每个函数都是闭包，每个函数天生都能够记忆自己定义时所处的作用域环境。但是，我们必须将这个函数，挪到别的
作用域，才能更好的观察闭包。这样才能实验它有没有把作用域给“记住”。
因为我们总喜欢在函数定义的环境中运行函数。从来不会把函数往外挪。那为啥学习闭包，防止一些隐患。

```js
函数嵌套函数
                    ( es6 : 父作用域 嵌套  子作用域)
                    内部函数  使用 外部函数的 参数或者 变量,构成闭包
                    被使用的  变量或者参数  不会被回收  -- 一直使用
                    
                    
			var a = 10 ;
              console.log(a);
             })(); 
               //这个 a 使用完了  就会回收
 
             (function(){
                 var a = 10 ;
                 document.onclick = function(){
                     console.log(++a);
                 }
             })();

            // 这里的  变量 a 就不会被回收

     闭包 影响是什么 ? 这个变量  或者 参数不会被回收

            // 只要你内部的函数被持续引用触发,
            // 就不会被回收,这个局部变量就不会被回收
   (function(){
                var a = 10 ;
                document.onclick = function(){
                    var a = 10;   
                    console.log(++a);
                }
            })();

            //  点击函数里边的a 这个a 就会被回收,这个时候一直打印的是  11



```

闭包案例：

```js
  			var ap = document.getElementsByTagName("p");
 <body>

		<p>11111111</p>
		<p>22222222</p>
		<p>33333333</p>
		<p>44444444</p>

		<script>
			var ap = document.getElementsByTagName("p");

			// 这里的  循环 点击是得不到序号的------>全部打印4
			for (var i = 0; i < ap.length; i++) {
				ap[i].onclick = function() {
					console.log(i);
				}
			}

			// for (var i = 0; i < ap.length; i++) {// a
			//    (function(i){ // b
			//         ap[i].onclick = function(){ // c
			//         console.log(i); // d
			//     }
			//    })(i);  // e
			// }

			//  a 和  e  i一样
			//   b ,c, d一样

//辅助理解------效果同[注释代码]上
			for (var i = 0; i < ap.length; i++) { // a
				(function(index) { // b
					ap[index].onclick = function() { // c
						console.log(index); // d
					}
				})(i); // e
			}

----------------------------------------------------------------------
			for (var i = 0; i < ap.length; i++) {

				ap[i].onclick = function(index) {
					return function() {
						console.log(index);
					}
				}(i)

			}
		</script>
```

