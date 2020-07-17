## <font style="color:Beige;">认识JS</font>

JS入门篇-----2020.03.09

------

#### 🍀变量声明：

#### 	<font style="color:skyblue;font-size:20px">关键词  var </font>-----声明 一个变量 ,开辟一块空间 ,用来存放东西 
		例：	var box = document.getElementById("box");
						// 这里的  box  就像一个  容器
			console.log(box);// 变量 js 可以识别 加了引号就是<font style="color:yellowgreen;font-">字符串 ,不是变量 </font> 
			box.innerHTML ="茜小妹儿";

​	 var goudan;
​			console.log(goudan); // undefined  ---未定义
​			goudan="狗蛋";
​			console.log(goudan); // 狗蛋

​	yanxin
​		   console.log(yanxin); // 不声明变量就使用 ,一定会报错

​	yanxin = "言心" // 不使用关键词  声明  会自动变成<font style="color:Olive;font-size:18px">全局变量</font>, 挂到 window里边去
​			console.log(window); // 声明的变量  会存在window 当中

🔺 总结声明变量的各种情况

```js
			var a;
			var b = 10;
			var c = 1+1;  // 运算赋值
			console.log(c);

		var d = 20;
		var d = 30;
		console.log(d);
		// 多次声明  没有意义  只以最后一次为准
		var e = 1 , f = 2 ,g = 10;  // 逗号  话还没有说完  还有下一句
		console.log(e,f,g);
		// var  定义多个变量  用逗号隔开
		
		var aa , bb , cc ;
		aa = 11;
		bb = 22;
		cc = 33;
		console.log(aa , bb , cc);
		// 先声明  后赋值
```
------

​	<font style="color:red;font-size:20px">变量的命名规则</font>（百度看一下  关键字 和  保留字 ）
					1. 严格区分大小写
					2. 见明知意
					3. 不能以数字开头,只能包含字母 数字 _ $ 
					4. 不能使用中文
					5. 不能使用 关键字 和 保留字
					

```js
			关键字 === 已经赋予特殊功能的字
			保留字 === js预定可能在未来要使用的字
				
				tip: top 也是保留字
			
			变量命名规范: 驼峰命名法 : oBox (单个) ,aList (集合) 
			常量命名规范: 全字母大写加下划线拼接 : MAX_NUMBER 
			// 后期不能修改  只能读取  --  常量一旦定义好,就不能改变,拿过使用 
```
------

操作html页面:
   1.    获取标签
         		如何获取:
         			css -- 选中标签 ---  id class 标签名 ...
         			js --- 选中标签 ---  id  class 标签名  ...

         <div id="box">这是一个盒子</div>

          document.getElementById("box");
          根节点 document 提供了操作其他节点的方法   .  的  谁的 --- 属于谁
          console.log(document.getElementById("box")); // 返回 id 名 box 的元素

         ​	<font style="color:yellow;font-size:20px">操作元素的内容  :  innerHTML  / innerText</font>
         ​		  ⭐ 这里的   =  这个符号 叫  赋值   意思: 右边的赋值给左边
         ​			document.getElementById("box").innerHTML = "罗滔的媳妇是小珂班长";
         ​			document.getElementById("box").innerHTML = "<p>罗滔的媳妇是小珂班长</p>";
         ​			// innerHTML 会解析标签
         ​			document.getElementById("box").innerText = "你好啊";
         ​			document.getElementById("box").innerText = "<a href=''>大家好</a>";
         ​			//  innerText 不会解析标签 

         ------

         es5 数据类型 -- 6
         				es6 新增一个基础数据类型
         				es5 数据类型 -- 6
         					基础数据类型  -- 5
         					

         ```js
         					number -- 数值型
         					String -- 字符串 
         					boolean -- 布尔值 : true 正确  / false 否  -- 判断 / 开关
         					null -- 空 -- 含义 -- 空对象
         					undefined  --- 未定义
         					
         				复杂数据类型  -- 1
         				
         					object -- 对象 
         						
         ```

         ```js
         			检测数据类型: typeof
         				typeof  使用方法 
         					typeof 变量名
         					typeof(变量名)
         				特殊:  
         					typeof null  -- object
         ```

例：

```js
			var a = 12.5;
			var b = 0/0;
			//console.log(b) // NaN  -- not a number   坏掉的数
			var c = " 言心 12 32 "; // 引号包裹的内容都是字符串
			var d = ""; // 空字符串
			var dd = '';// 空字符串
			console.log(typeof dd)
			 
			 
			var bool = false;
			console.log(typeof bool) 
			// if(bool){
			// 	alert(1);
			// }
			
			var yanxin = null;
			console.log(typeof yanxin) 
			// null 为了保证类型统一的  定义好的变量将来用于保存对象,最好将变量初始化为null
			
			var maomao ;
			var maomao = undefined;
			console.log(typeof maomao) // undefined   无意义，多此一举了
```

