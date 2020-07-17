## 	<font style="color:pink">Js操作标签的属性</font>

​						js05---2020.03.13

------

#### ⭐	<font style="color:Turquoise;">鼠标事件: </font>

##### 			onclick  点击事件  ***

##### 					鼠标移入

​			onmouseover  *
​					onmouseenter

##### 					鼠标移出

​			onmouseleave * 
​					onmouseout

##### 					鼠标按下 / 抬起

​			onmousedown
​					onmouseup

##### 					鼠标移动

​					onmousemove

------

🌏<a href="https://www.cnblogs.com/zzmb/p/7731970.html" style="text-decoration:none;">[JS中获取元素使用getElementByID()、getElementsByName()、getElementsByTagName()的用法和区别] )</a>

```js
<body>
		<div id="wrap"></div>
		<script type="text/javascript">
			var oWrap = document.getElementById("wrap");
			oWrap.onmousedown = function (){
				oWrap.innerHTML = "好好学习";
				// alert("加油！");
				console.log(1)
			}
			// oWrap.onmouseup = function(){
			// 	oWrap.style.backgroundColor = "pink"
			// }
			
		</script>
</body>
```

------

🌳标签的样式  : 是由我们的 层叠样式表 , 通过 stye
				外部样式: 
						js只能操作我们当前这个html,对于<font style="color:DeepPink;">html之外</font>的的文档是操作不了的
					内部样式:
						可以但不常用,不好掌控---------选择器优先级问题,还有修饰不好看
					🌝行内样式:
							<font style="color:gold;">这个是我们通常使用的 js操作样式的方法</font>

```js
<body>
		<div id="wrap"></div>
		<p id="p1">zzy洋</p>
		<script type="text/javascript">
			var oWrap = document.getElementById("wrap");
			var oStyle = document.getElementById("sty");
				// oStyle.innerHTML = "p{color:blue;}"
			oWrap.style.backgroundColor = "pink";  // 添加到 行内样式
		</script>
</body>
```

------

<font style="color:OrangeRed;font-size:20px">🔺标签的属性操作：</font>

```html
<body>
		<a href="http://www.baidu.com" id="wrap" class="aa" yanxin="言心">付出不亚于任何人的努力</a>
		<script type="text/javascript">
			
			var oWrap = document.getElementById("wrap");
			console.dir(oWrap); 
			// 操作标签属性  直接---->点" . " 操作
			
			// 获取标签的属性
			// console.log(oWrap.href)
			// 修改标签的属性值
			oWrap.href = "http://www.taobao.com"//网站从百度更换为淘宝
			
	添加自定义属性
			// oWrap.yanxin = "言心";
			// console.dir(oWrap);
			// 注意: 已经有很多存在的属性,你在自定义的时候 别和人家的冲突了
			// 修改了  会影响人家的一些功能
			
			// class 不一样  class是关键字  通过className  代替
			oWrap.className = "xiaoou"
			
    操作自定义标签的属性  
			// oWrap.yanxin = "小妹";  不可以直接 
			// 存在哪?  --- >attributes  一层一层的找  nodeValue  里边
			oWrap.attributes[3].value = "小妹"
			// vue  v-bind  
			
			/*
				三种方法 操作标签属性:  一般不操作合法属性
				getAttribute();
				setAttribute();
				removeAttribute();
			*/
			console.log(oWrap.getAttribute("yanxin"))
			oWrap.setAttribute("yanxin","阿福")
			oWrap.removeAttribute("class") 
			
		</script>
</body>
```

------

### 🍀<font style="color:LightPink">样式操作的代替方案：</font>

代码例：

```css
<style type="text/css">
			*{margin:0;padding:0;}
			div.aa{
				width: 200px;
				height: 200px;
				margin: 100px;
				border:5px solid #008000;
			}
			
			div.show{
				font-size:25px;
			}
</style>
```

```js
<body>
		<div id="wrap" class="aa">付出</div>
		<script type="text/javascript">
			/*
				style  规定   行内样式
				
				代替方案:  最合理的方案
					提前写好css样式  -- 优先级做自己控制  -- 操作 class 的名字
			*/
			var oWrap = document.getElementById("wrap");
			console.log(oWrap.className);
			oWrap.onclick = function (){
				// oWrap.className = "aa show"
				// 拼接
				//oWrap.className = oWrap.className + " show"
				// 换种写法 +=  在原有的  后边  再追加  一个
				// oWrap.className += " show"
				
				
				😙 HTML5 标准下 DOM 提供一个新的 API  classList  操作class类名的
				
				oWrap.classList.add("show")
				// oWrap.classList.remove("aa")
				// console.log(oWrap.className);
			}
			console.log(oWrap.className + " show")
			console.dir(oWrap)

console.log()会在浏览器控制台打印出信息
console.dir()可以显示一个对象的所有属性和方法
			
			// 特殊的  :   ie8  css  标准浏览器   style
			oWrap.style.styleFloat="right"
			
			// 获取  元素的  显示样式:  compured  操作
			// 获取某个元素节点的 样式
			console.log(getComputedStyle(oWrap))
			
		</script>
```

------

#### 🔸	<font style="color:gold;font-size:20px">静态获取:		只会在调用获取元素的时候获取一次</font>

​             querySelectorAll
​                    querySelector
​                    getElementById
​	<font style="color:hotpink;font-size:20px">querySelectorAll与querySelector的区别:</font>

```js
 querySelectorAll : 找出所有匹配的节点并返回数组.
1、querySelector只返回匹配的第一个元素，如果没有匹配项，返回null。 
2、querySelectorAll返回匹配的元素集合，如果没有匹配项，返回空的nodelist(节点数组)。 
3、返回的结果是静态的，之后对document结构的改变不会影响到之前取到的结果

找出body标签下的第一个div
document.body.querySelectorAll("div")[0]

找出所有标签 
document.querySelectorAll("*")

找出head下所有的标签 
document.head.querySelectorAll("*")

找出所有class=box的标签 
document.querySelectorAll(".box")

找出所有class=box的div标签 
document.querySelectorAll("div.box")

找出所有id=lost的标签 
document.querySelectorAll("#lost")

找出所有p标签并且id=lost的标签 
document.querySelectorAll("p#lost")

找出所有name=qttc的标签 
document.querySelectorAll("*[name=qttc]")

找出所有存在name属性的标签 
document.querySelectorAll("*[name]")

找出所有class=hot并且存在name属性的p标签 
document.querySelectorAll("p.hot[name]")
document.querySelectorAll("p[class=hot][name]")

```



```js
               // 除了以上三种之外都是动态获取

🔸动态获取: 每次使用到的地方都会重新调用获取元素的方法
                getElementsByTagName
                getElementsByClassName
                getElementsByName
```

```js
 <body>
        <ul id="list">
            <li>1</li>
            <li>2</li>
            <li>3</li>
        </ul>

        <script>
           
            var oUl = document.getElementById("list"),
                // aLi = document.querySelectorAll("li");
                aLi = document.getElementsByTagName("li");
            
            // oUl.onclick = function(){
            //     oUl.innerHTML += "<li>4</li>";
            //     console.log(aLi.length);
            // }

            
            oUl.onclick = function(){
                oUl.innerHTML += "<li>"+(aLi.length+1)+"</li>";
                console.log(aLi.length);
            }

        </script>
    </body>
```

------

​	<font style="color:tomato;font-size:20px">innerHTML谨慎使用</font>

```js
 <body>
        <div id="box">
            <p id="txt">123</p>
        </div>

        <script>
            var box = document.getElementById("box");
            var txt = document.getElementById("txt");

            box.innerHTML += "<p>456</p>";
            /*
                += 的原理
                a += b

                a = a + b

                赋值的过程?  原来的值去哪里了? 被覆盖了

                不再是原来的那个 txt

                可以在 += 之后获取

            */
            
            txt.innerHTML = "789"
            
        </script>
    </body>
```

