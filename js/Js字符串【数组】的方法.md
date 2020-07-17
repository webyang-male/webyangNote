## <font style="color:pink;">Js数组的方法</font>

--2020/04/01

------

### splice()  -- 影响原始数组的

> ​              参数1: 从哪个位置开始
> ​                     参数2: 删除几个 (为零的时候)--添加
> ​                     参数3: 用什么数据替换

```js
		 var arr = ["小婷","小欧","小妹","小雷"];

            // 删除 
            // console.log( arr.splice(1,3) ); // 删除的数据 (数组包裹起来)
            // arr.splice(-1,2)  // 倒着删除  
            
            // 添加 可以添加多个
             var vv = arr.splice(1,0,{x:1});
             console.log(vv); // 返回值是空数组
            
            // 替换  可以替换多个
  		   var arr = [1,2, 3, 4];
            var x = arr.splice(1,1,"毛毛","二毛")
            console.log(x);
            console.log(arr);
            //[2]
		   //(5) [1, "??", "??", 3, 4]

```

------

###    数组排序:

####             sort--正序

#### 			reverse() -- 反序

```js
	    var arr = [1,5,8,7,6,4,3,2,9];
----------------------------------------------------------------
            var arr2 = ["a","c","ae","ab","e"]
            // 排序
           var a = arr2.sort();
       console.log(a);  //(5) ["a", "ab", "ae", "c", "e"]
            
            // 接收函数的返回值,返回值(正负)决定了 是否排序
            var c = arr.sort(function(a,b){
                // a - b-- 正序
                // return a-b;
                // 倒叙  b-a
                return b-a;
            });
            // console.log(c);
    --------------------------------------------------------------------        
            var arr1 = [
                {name:"灵逝",age:18},
                {name:"班长",age:12},
                {name:"小欧",age:19},
                {name:"言心",age:30}
            ]

            arr1.sort(function(a,b){
                return b.age -a.age ;
            })
            console.log(arr1);
            
//结果如下图
```

![image-20200403190701446](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200403190701446.png)

------

###  Slice 

```js
 /*
                slice   可以用到数组里边   切割

                不会影响原始数组
            
            */
            var  arr = [1,2,3,4];
---------------------------------------------
            // 不传参 --- 返回完整的数组
            var a = arr.slice()
            console.log(a);//(4) [1, 2, 3, 4]
            
            // 只有一个参数  返回该参数指定位置开始到当前数组末尾的所有项
            var b = arr.slice(2);
            console.log(b);
           console.log(arr);
// (2) [3, 4]
// (4) [1, 2, 3, 4]
            
            // 两个参数 返回起始位置和结束位置之间的项  左闭右开
            var c = arr.slice(0,2);
            console.log(c);
```

------

### join

把数组按照规则  拼接成  字符串   ,不改变原数组

```js
  		   var arr = ["小妹","无痕","婷婷","忘记"];
            var a = arr.join("-")
            console.log(a);//小妹-无痕-婷婷-忘记
            
            // 判断是否为数组 Array.isArray()
            var b = 12;
            var c = "你好";
            var d = [1,2,3]

            console.log(Array.isArray(b)); // false
            console.log(Array.isArray(c)); // false
            console.log(Array.isArray(d)); // true
            

```

------

### Concat合并

```js
 // concat()
            var arr1 = [1,2,3]
            var arr2 = [7,8,9]

            
            // 合并两个数组为一个新的数组
            var arr = arr1.concat(arr2);
            console.log(arr);//(6) [1, 2, 3, 7, 8, 9]
            
```

------

### 常用遍历方法

```js
		 // forEach() 数组的遍历循环
            var arr = [1,2,3]

            // for  不直观
            // for (let i = 0; i < arr.length; i++) {
            //     console.log(arr[i]);
            // }

            //代替  
            /*
                forEach()  括号里边是函数
                函数里边的传参
                    1. 数组的项
                    2. 数组的项的下标
            */
          arr.forEach(function(val,key){
               console.log(val,key);
          });

            // 使用方法-- elemennt
            // 类数组
            var ap = document.querySelectorAll("p");

            // ele 存储的数据  i 就相当于下标
            ap.forEach(function(ele,i){

                ele.onclick = function(){
                    console.log(this.innerHTML);
                    
                }

            });

```

