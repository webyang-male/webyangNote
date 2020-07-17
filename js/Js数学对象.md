

## Js数学对象

​		----2020/04/03

------

### 改变 this 指向三种方法: 
### 	call              				

```js
 			function a(){
                console.log(this);

            }
            a();//this指向window
```

![image-20200406170142609](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200406170142609.png)

```js
function a(x, y) {
            console.log(x, y);
            console.log(this);
        }
        a();

 var obj = {
                name: "笑笑",
                age:16
            }
            // 指向这个对象  call 能在内部执行a函数
            // 第一个是this的指向,除过第一个,从第二个开始才是参数的第一个
            // 错开一位,然后一一对应传参
            a.call(obj,18,20); 
```

**Call案例：**

```js
//文字点击放大效果
<div id="wrap">小晓</div>
        <script>
            var wrap = document.getElementById("wrap");
            wrap.onclick = function(){
                changeSize.call(wrap,36)
            }

            function changeSize(size) {
                console.log(this);
                
                this.style.fontSize = size +"px";
            }
</script>
```

**Bind案例：**

```js
//文字点击放大效果
<div id="wrap">学习</div>
        <script>
            var wrap = document.getElementById("wrap");
            wrap.onclick = changeSize.bind(wrap,50);

            function changeSize(size) {
                console.log(this);
                this.style.fontSize = size +"px";
            } 
        </script>
```

**Apply案例：**

```js
   function a(x,y,z){
                console.log(x,y,z);//10 20 undefined
                console.log(this);// Window

            }
        a(10,20)



```

>  	call 和 apply 只是传参不一样
>    	     apply 传参需要放到一个数组中,数组中项和形参一一对应
>    	     a.apply(document,[10,20,888]

​     bind 不会帮你执行a函数,只会返回一个**新的函数**
​            console.log(a.bind(document));

```js
        传参数 可以   var b = a.bind(document,10,20,30);
```

```js
  		   var b = a.bind(document,10,20,20);
            b(); // 函数再次执行的时候 可以直接使用bind 之前绑定的各种内容
            // b(40,50,60);  // 也可以在这个里边传参数
```

------



### 随机数

```js
 
            console.log( Math.random() );  // [0,1)  的随机数   包括零 不包括1

            // 0~5 的随机数
            // [0,1)*5  === [0,5)
            console.log( Math.random()*5 );  

            // 15~25的随机数
            // [0,1) === [0,1)*10 === [0,10)+15  === [15,25)
            /*
                Math.random()*(max-min)+min ; 
            */
            console.log( Math.random()*10+15 );


            // (0,1] 之间的数 

            // [0,1)*-1   === [0,-1)+1 === [1,0) => (0,1] ---演示过程

            console.log( Math.random()*-1+1 );

```

### 整数方法

```js
 // 整数的方法
            
            // 四舍五入入
            console.log( Math.round(3.2333) );  // 3
            console.log( Math.round(3.55555555) );  // 4
            console.log( Math.round(-3.55555555) );  // -4
            console.log( Math.round(-3.0001) );  // -3



            // 向上取整  ceil  
            console.log( Math.ceil(1.00000001) );
            console.log( Math.ceil( -3.0001 ) );

            // 向下取整  
            console.log( Math.floor(6.0000010)  );
            console.log( Math.floor(6.999999999)  );
            
            // 0-10 随机整数  [0 , 10]

           console.log(  Math.floor( Math.random()*11 ) );
            
           //  5-10  [5,10]  ---- 0,1 *6 === 0,6 === 5-11
           console.log(  Math.floor( Math.random()*6+5 ) );

            // 绝对值
            console.log(   Math.abs(-0.1111)  );
            

            //最大数
           console.log(  Math.max(10,100,20,50,800) );
           
           // 最小数
           console.log(  Math.min(10,100,20,50,800) );

           // 指数
          console.log(  Math.pow(2,10) ); //  2的10次方
          
    
```


```js
       // 保留小数 
       console.log(  Math.random().toFixed(4) );
```
