## 	<font style="color:pink;">循环</font>

​	--------2020.03.20第13节

------

 for 循环:
                    语法: 
                    for (var i = 0; i<10 ; i++){ 代码块 }

 for循环:
                    存在的目的 ?
                        执行重复的 大量操作
                    控制什么?
                        循环次数 --- 判断(条件) --- 递增 or 递减的量
                    执行循序?
                        ① 初始变量的值  var i = 0;
                        ② 判断条件 i<10
                        ③ 干活-循环需要做的事情
                        ④ 递增 or 递减的量  i ++

🌿 循环的关键字:

​		break --- 强制退出循环体,后边的循环不在执行
​                continue --- 跳出当前的循环体,进入下一次循环

```js
                for( 1初始表达式 ; 2条件 ; 3增量表达式 ){ 4循环需要做的事情  }

                ()里边的语句用  ; 隔开
 // 执行顺序:  1  - >  2 -> 4 -> 3
<body>
        <!-- <ul>
            <li>1</li>
            <li>2</li>
            <li>2</li>
        </ul> -->
        <div id="wrap"></div>
        <script>
            // 大量的  重复性的工作
            /*
                js--- 循环--多次执行 同一个代码块
            */
            var li = document.getElementsByTagName("li");
            // li[0].onclick = function(){
            //     alert(1)
            // }
            // li[1].onclick = function(){
            //     alert(1)
            // }
            // li[2].onclick = function(){
            //     alert(1)
            // }
            
🌞for循环第一种写法（推荐）
            // for( var i = 0;i < 10 ; i++ ){
            //     console.log(i)
            // }
            // 执行顺序:  1  - >  2 -> 4 -> 3
-----------------------------------------------------------
		第二种
        	输出01234
            // var i = 0;
            // for( ; i<5 ; ){
            //     console.log(i);

            //     i++; // 放在所有的语句之后
            // }


            var wrap = document.getElementById("wrap");
            
            // wrap.innerHTML += "<p>1</p>"
            // wrap.innerHTML += "<p>2</p>"
            // wrap.innerHTML += "<p>1</p>"
            
            var h = "";

            for (var i = 1 ; i <= 1000 ;i++) {
                h += "<p> 序号: "+ i +"</p>"
            }

            wrap.innerHTML = h;
            
            // js 浏览器  js  -> html+css


        </script>
    </body>
```
------

##### break,continue

```js
<script>
          
            // break  continue
            
            /*
            for (var  i= 0; i < 10 ; i ++) {
               if( i===5 ){
                   //break; // 跳出循环,结束循环
                   continue; 
                   // 跳过当前的循环继续新一轮的循环
                   // 此次循环的后续代码不在执行 ,直接进入 语句4  i++ ;
               }
               console.log(i);
            }
            */
            /*
                0 --->  0
                1 ---> 1
                2 ---> 2
                3 ----> 3
                4 ---->4
                5 = 5

            */

            var a = 5;
            for( var j = 0; true ; j++ ){

                if ( j === a ) {
                    break;
                }
                console.log(j);
            }


        </script>
```



------

##### 嵌套for循环

```js
<script>
            
            for( var i = 0; i<10; i++ ){

                console.log("茜小妹儿" +i );

                for( var j = 0; j < 5 ; j++ ){
                    console.log("言心");
                }
                
            }
        </script>
```

------

##### while循环

```js
 <script>
           //   while 声明促使变量在外面 ,条件在括号内, 变量的变化在代码块内
            
            var b = 0;
            while( b < 5 ){
                console.log("我是 while "+  b);
                b++;
            }

            // 不管你符不符合条件,先上来做一次,在进入判断
            var v = 5;
            do{
                console.log("我是 do while "+ v);
                v++;
            }while(v<5);

        </script>
```

