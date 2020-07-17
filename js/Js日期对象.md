## Js日期对象

---2020/04/07

------



#### 1.  什么是日期对象

日期对象是JS中提供的用于控制年份.日期.时间.的对象



#### 2.  创建日期对象

Date是js内置的构造函数,使用前,先通过new操作符构造一个日期对象出来

```js
var date = new Date();  
console.log(date);   // Fri Apr 26 2019 15:14:55 GMT+0800 (中国标准时间)
```

注意,这个获取时间不是实时的,是将此时此刻你获取的时间拍照报存在变量date中



#### 3.  优化获取的时间

> date.toLocaleString()		==>	获取本地日期字符串
> date.toLocaleDateString()	==>	获取本地(年/月/日)形式的字符串
> date.toLocaleTimeString()	==>	获取本地(时/分/秒)形式的字符串

##### 3.1  本地日期格式

```js
date.toLocaleString();     // "2019/4/26 下午3:17:33"
date.toLocaleDateString(); // "2019/4/26"
date.toLocaleTimeString(); // "下午3:17:33"
```



#### 4.  日期对象的方法

##### 4.1 日期对象的获取方法

> date.getFullYear()			==>获取年份		
>
> date.getMonth()			==>获取月份		(0-11)
>
> date.getDate()			==>获取日		(1-31)
>
> date.getHours()			==>获取小时
>
> date.getMinutes			==>获取分钟
>
> date.getSeconds			==>获取秒钟
>
> date.getMilliseconds			==>获取毫秒
>
> date.getDay()		==>获取星期几		(0-6)	星期天是0
>
> date.getTime()		==>获取当前时间到1970年1月1日午夜的毫秒数
>
> date.getTimezoneOffset()	==>	获取当前时区的偏移时间  东8区是-480

```js
// 1. 获取年份
date.getFullYear();   //2019

// 2. 获取月份
// 注意,月份的计数是从0开始的的,也就是1月份的数字为0,
// 所以获取月份数字后加1才是真正的月份
date.getMonth() + 1;    // 3 + 1 = 4  ,

// 3. 获取日期
date.getDate();   // 26

// 4. 获取小时
date.getHours();      // 10

// 5. 获取分钟
date.getMinutes();      // 30

// 6. 获取秒数
date.getSeconds();      // 20

// 7. 获取毫秒数
date.getMilliseconds();      // 862

// 8. 获取星期几
date.getDay();         //  5

// 9. 获取时间戳
date.getTime();       //1556264444841(1970年1月1日0时0分0秒到现在的毫秒数)

// 10. 获取格林威治时间与本地时间相差的分钟数
date.getTimezoneOffset();  // -480 时区便宜,我们是东八区
// 查看格林威治时间
date.setHours(date.getHours() + date.getTimezoneOffset()/60);
date.toLocaleString();
```



##### 4.2 设置时间的方法

> date.setFullYear()			==>设置年份		
>
> date.setMonth()			==>设置月份		(0-11)
>
> date.setDate()			==>设置日		(1-31)
>
> date.setHours()			==>设置小时
>
> date.setMinutes()			==>设置分钟
>
> date.setSeconds()			==>设置秒钟
>
> date.setMilliseconds()			==>设置毫秒

```js
// 1. 设置日期的年份
date.setFullYear(2017);     // 返回时间戳

// 2. 设置月份
// 注意如果你传入的参数大于11,将会减掉11,加一年,然后身下的在修改月份
date.setMonth(10);         // 返回时间戳 
```



##### 例子:

每隔一秒将当前时间打印子控制台上

```js
setInterval(function(){
    var date = new Date();
    var YY = date.getFullYear();   //2019
    var MM = date.getMonth() + 1;
    var DD =  date.getDate();  
    var hh = date.getHours(); 
    var mm = date.getMinutes();    
    var ss = date.getSeconds();  
    console.log('现在的时间时'+YY+'年'+MM+'月'+DD+'日'+hh+'时'+mm+'分'+ss+'秒');
},1000)
```





#### 5. 获取指定的时间戳

> var date = new Date(参数)	==>获取指定时间戳
> 参数形式				       ==>2019,1,5 2019年2月5日

```js
// 获取指定的时间戳
var date = new Date(2019,1,5)	
```



#### 6. 获取两个时间戳相差的毫秒数

两个时间戳相减是毫秒数

```js
var date = new Date(2020,0,25);
var now = new Date();
var _date = date - now;  	// _date的值是毫秒数
```



##### 6.1 距离过年的时间

```js
// 距离过年的天,时,分,秒
setInterval(function(){
    var day = Math.floor(_date/(1000*60*60*24));
    var hours = Math.floor(_date/(1000*60*60)%24);
    var minutes = Math.floor(_date/(1000*60)%60);
    var seconds = Math.floor(_date/(1000)%60);
},1000)

// toTwo
function toTwo(num){
    if(num < 10){
        return '0' + num;
    }else{
        return num;
    }
}
```

**距离生日（毫秒）**

```js
var date = new Date();

    // 2000 .2.29

    var birthday = new Date(2000,2,29)

    console.log( date - birthday)
```



#### 7. 验证定时器的时间不准

```js
var start = new Date();
setInterval(function(){
    var now = new Date() - start;
    console.log(now)
},1000)
```



