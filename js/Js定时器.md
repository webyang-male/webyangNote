# 	<font style="color:hotpink;">定时器</font>

---2020/04/03

------



## 一.定时器

#### 1. JS存在两种定时器

> setTimeout()		延迟定时器
>
> setInterval()		 循环定时器(“间隔器”)

定时器中的函数挂载在window对象,内部的this  ---->  window

```js
setTimerout(function(){
    console.log('wuwei')
},1000);   // 一秒后打印wuwei

setInterval(function(){
    console.log('wuwei')
},1000);   // 每隔一秒就打印wuwei,如果不清楚或关闭页面,将会无限循环下去
```



#### 2. 清除定时器

⚠️在每次使用定时器时,都必须清除定时器

如何清除定时器呢,每一个定时器开启后都会返回一个对应的id,说白了就是js里面的第几个定时器,通过这个id就可以清除定时器,清除定时器用如下的方法

> clearTimeout(timer)      ==>  用于清除setTimeout
>
> clearInterval(timer)       ==>  用于清除setInterval

```js
// 在开启定时器的同时定义一个变量接受定时器返回的id,用于清除定时器

// 清除setTimeout
var timer = setTimeout(function(){
    console.log('wuwei')
},2000)
clearTimerout(timer);

// 清除setInterval
var timer2 = setInterval(function(){
    console.log('wuwei')
},2000)
clearTimerout(timer2);
```



#### 3.关于定时器函数的参数

定时器接受多个参数

> 1. 第一个参数是执行的函数,必须传递,不传没什么意义,不传报错
> 2. 第二个为定时器执行的毫秒数,可以不传,不会报错,感觉起来是理解执行,其实不是
> 3. 第三个之后的所有参数,都将是第一个参数函数执行的实参

```js
// 省略第二个参数会立即执行
setTimeout(function(){
    console.log('无为')
})

// 第三个以后的参数是第一个参数函数执行时的实参
setTimeout(function(,a,b){
    console.log(a,b)
},2000,10,20);
```

HTML5标准规定了setTimeout()的第二个参数的最小值（最短间隔），不得低于4毫秒，如果低于这个值，就会自动增加。在此之前，老版本的浏览器都将最短间隔设为10毫秒。不同的浏览器实现不同



#### 4. 关于定时器的第一个参数

定时器的第一个参数可以是多种形式

##### 4.1 匿名函数(常用)

```js
setTimeout(function(){
    console.log('aa')
},2000)
```



##### 4.2 有名函数

```js
setTimeout(function haha(){
    console.log('bb')
},2000)
```



##### 4.3 有名函数的函数名(常用)

```js
function haha(){
    console.log('cc');
}
setTimeout(haha,2000)
```



##### 4.4 能被当成js语句执行的字符串

```js
setTimeout('console.log("hahah")',2000)
```





### 二.JS的单线程

#### 1. 什么是单线程

​	<font style="color:yellow;">同时只能执行一个任务，如果后面有任务，需要等到当前任务执行完毕，才能执行后面的任务</font>

```js
for(var i = 0; i< 10000;i++){
    console.log(1);
}
console.log(2);   
```

js的是单线程的也就是说上面的代码,无论for循环多么耗时,都必须**先执行完**,在打印2,这就会造成线程阻塞的问题



#### 2. 什么叫线程阻塞

前面的任务死循环或者耗时过长导致后面代码不能被执行，这种情况叫做线程阻塞,

这样就会出现问题,所以对于像事件,定时器,ajax请求这种非常耗时的程序.浏览器就会开辟其他的线程来处理,所以这些程序我们叫他异步程序

浏览器有三个常驻线程,JS引擎线程,界面的渲染线程,已经浏览器事件触发线程,还有一些执行完就终止的线程,比如HTTP请求线程

在看一个例子:

```js
for(var i = 0; i< 5; i++){
    setTimeout(function(){
        console.log(i)
    },1000)
}
// 打印几
```





#### 3. 同步任务和异步任务

js的单线程意味着所有的任务需要排队,前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

所以JS的任务队列分成两种,同步任务,和异步任务,

> 1. 同步任务：在JS主线程排队执行的任务                
>
> ​		 =>	形成执行栈
>
> 2. 异步任务：不进入主线程进入”任务队列”的任务  
>
> ​		=>	形成任务队列

异步任务只有任务队列通知主线程,某个异步任务可以执行了,该任务才会进入主线程执行



所以下面的程序定时器会后执行

```js
setTimeout(function(){
    console.log(1)
},0)
for(var i = 0; i< 10000;i++){
    console.log(2)
}
```

定时器会等待for循环执行完毕后在执行.



#### 4. JS代码的执行顺序

先执行主程序的任务，当主程序的所有任务都执行结束，再把任务队列中的任务放到主程序中执行

> 1. 所有同步任务都在主线程上执行，形成一个执行栈。
> 2. 主线程之外，还存在一个"任务队列"。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
> 3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
> 4. 主线程不断重复上面的第三步。

### 堆和栈

 基本数据类型:  -- 存在栈内存中的

```js
                number  string  null  boolean  undefined 
                栈内存: 
                    值和值是相互独立的
                    修改一个不会影响另一个

            引用数据类型: 存在堆内存

                object

                变量是保存在内存地址中的,通过内存地址指向堆内存
```
```js
  var a = 10 ;
            var b = a ;
            a++;
            console.log(a + "------" + b);  // 11    10 
            

            var obj = {
                num:20
            }
            var obj2 = obj;
            obj2.num = 30;
            console.log(obj.num  + "------" + obj2.num ); //30------30

            console.log(obj === obj2);//true 引用相同地址
            
            obj3 = {age:18}
            obj4 = {age:18}
            
            console.log( {age:18} === {age:18});//false 不同对象


```

```js
var aa = [1,2,3]
 var aaa = [1,2,3]
            aa.forEach(function(v){
               v = v*20;
            });
            console.log(aa);  //aa数组--属于内置对象-- 数值不会改变

aaa.forEach(function(ele,i,arr){
               if(ele ===3){
                   arr[i] = 33;
               }
            });
            console.log(aaa);  // (3) [1, 2, 33]


```

改变原数组  ---- 通过<font style="color:red;font-size:20px">下标取值</font>进行修改

```js
  var bbb =[{age:18},{age:19},{age:20}];

            bbb.forEach(function(v){
               if(v.age === 18){
                   v = {
                       age:100
                   }
               }
            })
            console.log( bbb);//bbb未改变值
```

###   角度  和  弧度

```js
				 //360°  = 2π  弧度

                    //Math.PI  --- π
            console.log( Math.PI*2 );
            
            console.log( Math.sin( Math.PI/180*30 )  );
            console.log( Math.sin( Math.PI/180*60 )  );
            console.log( Math.sin( Math.PI/180*90 )  );
```

