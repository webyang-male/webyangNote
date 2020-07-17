## 运算符

​     js05 -----2020.03.14

------

- [x] =：赋值运算符
  ==：判断是否相等：忽略了类型进行值的比较。
  ===：判断是否相等：先进行值的比较，如果值相等，再去比较类型。即带有类型的值的比较。

```js
    运算符左右最好有空格  --- 清晰
    一元运算符:  + -  ++ --
        1. +  
            放在  数值  前,不会产生任何影响. 
            放在  非数值 前, 会调用 Number()  -- 转成数值

        2. -  主要用来表示 负数
            放 数值  前 ,表示负数
            放在  非数值 前,会调用 Number()  -- 转成数值,并将其转换成负数
        
        3. ++  递增
            a++ 后++ , ++a  前++
                a++  -- >相当于 a = a + 1
                ++a  -->相当于 a = a + 1

                a++ 和 ++a 之间的区别: 
                    1. a++ 参与运算  -- 不会先自己+1, 运算完了之后,在给自己 +1
                    2. ++a  始终先想到自己,自己先吃了再给别人----- 参与计算时自己先+1，然后在运算
        
        4. -- 递减  同上
            a-- 后-- , --a  前--
                a--   -->
                --a   -->
```
```js
 		     var aa = 12;
            // var bb = 1 + aa ++ ;
            // console.log(bb); // 13  

             var a = 2;
            // console.log(a++);  // 2   
            // console.log(a);

             var qq = 22;
           console.log(++qq);//23

           var ww = 22;
            // ++ ww;
            // console.log(ww);//23
            
            var ss = 22;
            // var ee = ++ss + ss++;
					(22+1)+23
            // console.log(ee);//46
            
            var cc = 22;
            console.log(cc--);

            var cc = 22;
            cc--;
            console.log(cc);

            var vv = 22;
            var gg = ++vv + vv-- + ++vv + --vv;
            //       23      23     23     22
            console.log(gg)

```

------

### 二元运算符:

####                     加法(+)  减法(-)  乘法(*)  除法(/)  求余(%)

加法  -- 拼接  
  					 两边任意一边有字符串,就是拼接 

减法 : 
   1.  都是数值  -- 正常计算

   2.  有一个是 字符串,布尔值,null,undefined,对象  -- 调用 Number()
       进行计算   如果调用 Number() -- NaN  -- 结果就是 NaN 

       乘法：

         		如果有一个数不是数值 ,调用 Number()转换成数值  -- 计算

除法 ：

​		有一个不是数值 ,调用 Number()转换成数值  ,有一个是 NaN 结果及时 NaN
​               被零除 -- Infinity   / -Infinity  ( 正负 取决于 前边的 符号 )

```js
		   var aa = 22 + 22;
            console.log(aa) //44

            var bb = 22 + "22";
            console.log(bb) //2222

            var cc = 22 + "{x:1}";
            console.log(cc) //22{x:1}

            var dd = "22" + "33";
            console.log(dd) //2233
           
            var ww = 22 + true;
            console.log(ww) //23

            // 有一个是 对象  , 调用他们的  toString()  -- 相对应的字符串
            var qq = 22 + [1,22];
            console.log(qq)//221,22

            var ff = 22 + {};
            console.log(ff)//22[object object]

            // 调用  Number()
            var gg = 22 + undefined;
            console.log(gg)//NaN

            var hh = 22 + null;
            console.log(hh)//22
```

