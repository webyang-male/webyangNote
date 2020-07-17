## Event事件

​	--2020/04/18

------

### 冒泡:

**可形象理解为游泳在水里边吐泡泡 ---从里边到外边**

```js
 <div id="box">
        <p></p>
    </div>
    <script>
        let box = document.getElementById("box");
        let p = document.querySelector("#box p");

        // box.onclick = function(){
        //     console.log("box被点击了");
        // };
        // p.onclick = function(){
        //     console.log("p标签被点击了");
        // };
        // 点击p标签,box也会触发---这个就是冒泡 向上传递

        // box.onmouseenter=function(){
        //     console.log("移入事件");  // 不会冒泡
        // };

        box.onmouseover=function(){
            console.log("移入事件");
        };

```

### 阻止冒泡:

```js
<div id="box">
        <p>姓名</p>
        <ul>
            <li>小a </li>
            <li>小小</li>
            <li>学习</li>
        </ul>
    </div>
    <script>
        let box = document.querySelector("#box ul");
        let p = document.querySelector("#box p");

        p.onclick = function(e){
            box.classList.add("on");
            e.stopPropagation(); // 阻止冒泡,事件到此为止,不会向上传递
        };
        // 点击别的地方,ul移出类名
        document.onclick=function(){
            box.classList.remove("on");
        }
```

### 默认事件：

#### 			oncontextmenu右键点击事件

```js
document.oncontextmenu = function(){
  console.log(1111111);
};
```

**不想右键点击出现菜单?**
	  **阻止默认事件的触发**

```js
document.oncontextmenu = function(e){
    console.log(11111);
    // 阻止默认事件触发
    e.preventDefault();
}
```

**右键菜单创建**

```js
document.oncontextmenu = function(e){
    // 阻止默认事件触发
    e.preventDefault();
    // 创建 div
    let div = document.createElement("div");
    div.id = "menu";
    // 可视区的距离
    div.style.cssText = "left:"+e.clientX+"px;top:"+e.clientY+"px";
    document.body.appendChild(div);
}
```

> 上面案例：改变 menu 唯一的div
> 	可采用--->**单例模式 --- 设计模式** 

### Dom 二级事件

##### 			<a href="https://blog.csdn.net/qq_23389687/article/details/80166843" style=" text-decoration:none;"><font  style="color:hotpink;font-size:18px">		🌏DOM0和DOM2级事件博客</font></a>

#### 		Dom 0级事件

```js
<div id="box">你好</div>
<script>
let box = document.getElementById("box");

box.onclick =function(){
    alert(1111)
};
box.onclick =function(){
    alert(22222)//弹窗为这个
};
</script>
```

#### dom 2级事件

```js
.addEventListener(什么事件,函数)---这里的事件不需要on
```

```js
box.addEventListener("click",function(e){
    console.log(111);
    console.log(this)//this指向box
});
box.addEventListener("click",function(e){
    console.log(222);
});
// 以上方法不会覆盖[输出结果是111,222]

// 换一种写法，不需要另一个事件函数----清除
document.addEventListener("click",function(){
    console.log(2222222)
});
document.removeEventListener("click",function(){
    console.log(2222222)
});
 //地址不一样----没有移除


```

#### Dom 2事件  --- 功能-函数-单独写出来    目的: 方便后期清除

```js
function ev(){
  console.log(2222222)
}
document.addEventListener("click",ev);
document.removeEventListener("click",ev);
```

### 事件捕获  

**捕鱼 --- 从外边到里边**
				<font style="color:skyblue;font-size:20px">先捕获后冒泡 </font>

```js
<body>
    <div id="wrap">
        <div id="son1">
            <div id="grandson"></div>
        </div>
    </div>
    <script>
        // 事件捕获 ---- 捕鱼 --- 从外边到里边
        let ow = document.getElementById("wrap"),
            son = document.getElementById("son1"),
            gs = document.getElementById("grandson");


        // 很少用--了解
        // 第三个参数是 true---事件捕获(从外到里边),
        //  默认冒泡,false
        ow.addEventListener("click",function(){
            console.log("爷爷被抓了");
        },true);
        son.addEventListener("click",function(){
            console.log("爸爸被抓了");
        },false);
        gs.addEventListener("click",function(){
            console.log("儿子被抓了");
        },true);
    </script>
</body>
```

### 事件委托: 让别人做事

```js
<div id="box">
    <p>000001</p>
    <p>000002</p>
    <p>000003</p>
</div>
<script>
let box = document.getElementById("box");
let p = document.querySelectorAll("p");

// p.forEach(node=>{
//     node.onclick = function(){
//         console.log(this.innerText);
//     }
// });
// 函数执行--3次 --- 消耗内存

// 简化---事件委托
box.onclick = function(e){
  // e.target 事件源
  console.log(e.target);
  console.log(e.target.innerText)
}
// 开辟一块空间,存储一个函数

// ie8 不兼容
/*
*   e = e || window.event
*   var target = e.target || e.srcElement
* */
</script>
```

![image-20200419152240891](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200419152240891.png)