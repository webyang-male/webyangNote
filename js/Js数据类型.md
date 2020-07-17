## 	<font style="color:HotPink;">Js数据类型</font>

​				js03---2020.03.11

------

> 🍀es5  数据类型
> 					基础数据类型:
> 						number 
> 						string
> 						boolean
> 						null
> 						undefined
>
> ​	复杂数据类型:
> ​						object  --  对象
> ​						对象:分类
> ​							1.内置对象: 有ES标准定义的
> ​								比如 : string ,Math, Date ,function,array
> ​							2.宿主对象:由 js 运行的环境提供的对象 (目前来讲-浏览器提供的对象)
> ​								比如:DOM  BOM
> ​							3.自定义对象:开发人员自己定义的对象: {}

​			Js的对象: 一个或多个属性构成的属性集合.
​									属性之间没有顺序关系,每个属性之间都是以<font style="color:skyblue;">逗号隔开</font>
​									属性是由两部分构成: 属性名:属性值					

```js
		// 简化写法
		var yanxin = {
			name :"言心",
			age : 12,
			job : "讲师",
		    where is yanxin? : "湖南长沙市岳麓区潭州教育"
		};
```
代码例1:通过-----点.

```js
// 如何获取属性的值 ?  通过  点 操作 ---> 谁的
			console.log(yanxin.name);
			
			//修改属性的值
			yanxin.age = 16;
			console.log(yanxin.age);
			
			// 添加属性
			yanxin.dog = "豆豆";
			
			// 删除属性
			delete yanxin.job ;
			console.log(yanxin);
			
			// 特殊情况
		console.log(yanxin.where is yanxin?);  // 错误的
			
			 // 推荐使用  [] 形式 获取 属性的值 ,
			 // 用 . 会产生我们想象不到的错误,或者 不利于维护
			
			console.log(yanxin["name"]);
```

------

object  获取属性值方法2

tip：这里属性名加了-----<font style="color:DeepPink;font-size:20px">引号""。</font>

代码例2：

```js
<script type="text/javascript">
			// 这里的对象的属性名其实就是字符串 , 只能是字符串
			var yanxin = {
				"name" :"言心",
				"age" : 12,
				"job" : "讲师",
				"where is yanxin?" : "湖南长沙市岳麓区潭州教育"
			};
			// 注意
			// yanxin["yanxin"] ;//添加了一个属性   没有值 undefind
			// console.log(yanxin);
			
			// 获取属性值  通过  中括号 []  获取
			yanxin["name"]
			console.log(yanxin["where is yanxin?"])
			
	</script>
```

------

✎number小知识:

代码例：

```js
<script type="text/javascript">
			var a = 3.1415926 ;
			var b = 5e8;  // e 底数  10 
			var c = 5e-4;
			
			// 特殊数字  : 无穷大  正 / 负
			var d = -5e131413926
			
			// Infinity 无穷大
			// 无底洞
			
</script>
```

------

### 🌟数组：

​	代码例：

```js
<script type="text/javascript">
			// 数组对象 : []
			var arr = [1,"小欧",{x:1,y:2},33,55,[11,22,[102,103,104],33]]
			
			// 如何  获取  数组里边的  数据
			// 通过 通过数组的序号取值  下标 从 0 开始的  通过  [] 获取 中阔里边是 下标
			var b = arr[2];
			console.log(b);//输出结果=={x:1,y:2}

			// 获取数组的长度
			var len = arr.length;
			
			// 获取数组的最后一项  len-1 数组最后一项下标  length-1
			console.log(arr[len-1]);
		
			// console.log(arr[arr.length-1])
			
	var arr1 = arr[len-1][0];//输出11---获取到[11,22,[102,103,104],33]
	var arr2 = arr[len-1][2][1]//输出103---[len-1][2]获取到[11,22,[102,103,104],33]中的[102,103,104]
			
		</script>
```

------

### <font style="color:yellowgreen;">数字类型转换</font>

 <font style="color:LightCyan;font-size:20px">Number()</font>
		通过这个函数  ,可以把其他数据类型转换成数字

```js
<script type="text/javascript">
			
			var num_a = false;
			console.log(Number(num_a)) // 0 
			//   true  -- 1  false --- 0
			var num_b = null;
			console.log(Number(num_b)) // 0
			// null  --- 0 
			var num_c = undefined;
			console.log(Number(num_c)) // NaN
			// undefined  --- NaN 
			var num_d = "12"
			console.log(Number(num_d)) // 12
			var num_e = "12你好"
			console.log(Number(num_e))// NaN
			var num_f = "-12"
			console.log(Number(num_f)) //  -12
			var num_g = "001"
			console.log(Number(num_g)) // 1
			var num_h = [];
			console.log(Number(num_h)) // 0
			var num_i = {}
			console.log(Number(num_i)) // NaN 
			
			/* 
				进制
					10 进制 --( 0-9  满 9 进 1 )
					8 进制  -- 以0打头 (0-7 满7 进1)
					16 进制 -- 以0x打头 (0-9,a-f, 满f进1 )
			*/
			var aa = 070; //输出 十进制中的  56
			var bb = 0xaB;
			
			console.log(Number(0xd)) // 将其转换成相同大小的十进制整数值

			// 空字符串  --- 0 
			var cc = "";
			console.log(Number(cc));
			
		</script>
	</body>
```

