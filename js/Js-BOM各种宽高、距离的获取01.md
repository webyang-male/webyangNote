## 各种距离的获取

​						--2020/04/16

------



#### BOM--open(地址,打开方式)

```js
document.onclick = function(){
                window.open(
                    "http://www.baidu.com",
                    "_self"
                )
            }
```



#### scrollTo() 定义和用法

scrollTo() 方法可把内容滚动到指定的坐标。

语法
scrollTo(xpos,ypos)

参数	描述
xpos	必需。要在窗口文档显示区左上角显示的文档的 x 坐标。
ypos	必需。要在窗口文档显示区左上角显示的文档的 y 坐标。

```js
		document.onclick = function () {
                window.scrollTo(1000, 500);
            }
```

#### onscroll滚动事件

```js
 window.onscroll = function () {
                console.log("scroll")
            }
```

#### onresize定义和用法

onresize 事件会在窗口或框架被调整大小时发生。

语法
In HTML:

<element onresize="SomeJavaScriptCode">
JavaScript 中:

window.onresize=function(){SomeJavaScriptCode};

```js
window.onresize = function () {
                console.log("resize")
            }
```

------

### 可视区宽高:

```js
<body style="height: 2000px;">

    <script>
        // 可视区的宽高  -- 包含 滚条的宽高
        console.log( window.innerWidth );
        console.log( window.innerHeight );

        // 可视区宽高  -- 不包含滚动条的宽高
        console.log(document.documentElement.clientWidth);
        console.log(document.documentElement.clientHeight);
    </script>
</body>
```

### 元素宽高获取

```css
 <style>
 #wrap{
            overflow: scroll;
            width: 100px;
            height: 200px;
            padding:50px;
            margin: 20px 0 0 20px;
            border: 5px solid red;
        }
    </style>
```

```js
<body>
    <div id="wrap">
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
        好好学习
    </div>
    <script>
        let oWrap = document.getElementById("wrap");

        // 盒子的内容区宽高  ,不包括 border ,包括内容区+padding
         console.log(oWrap.clientWidth);
       	 console.log(oWrap.clientHeight);

        // 盒子的实际大小 -- 内容区+ padding + border
        console.log( oWrap.offsetWidth); // width + 左右的padding + 左右的boeder
        console.log( oWrap.offsetHeight);// width + 上下的padding + 上下的boeder

        //	  1.  没有滚动条:
        //        内容没有超出盒子:
        //        宽: width + padding
        //        高: height + padding
        //    2. 有内容超出,没有滚动条
        //        宽: width + padding
        //        高: 内容的height + 上padding
        //    3. 内容超出,有滚动条
        //        高: 内容的实际高度 + 超出隐藏内容的实际所占的高度
       
        console.log( oWrap.scrollWidth);
        console.log( oWrap.scrollHeight);
    </script>
</body>
```

### 元素相关的各种距离

```css
		#fa{
            position: relative;
            width: 300px;
            height: 300px;
            padding: 50px;
            margin: 50px 0 0 30px;
            border: 5px solid red;
        }
        #so{
            position: absolute;
            left:100px;
            width: 100px;
            height: 100px;
            margin:20px;
            border: 20px solid blue;
        }
```

```js
<body>
    <div id="fa">
        <div id="so"></div>
    </div>
    <script>
        let so = document.getElementById("so");
        console.log(so.offsetParent);
        // 获取元素到定位父级的距离 (不包括父级的border)
        // 包括元素的margin值,也会算在内部

        // 获取元素左边或者上边  到定位父级左边或者上边的距离
        console.log(so.offsetTop);
        console.log(so.offsetLeft);

        //  元素相对视口左上角 一些尺寸
        console.log(so.getBoundingClientRect())
    </script>
</body>
```

![image-20200418234006347](D:%5C%E6%BD%AD%E5%B7%9E%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200418234006347.png)

### 滚动高的获取

```js
body {
            user-select: none;
        }
<body style="height: 2000px;">

    <script>
       // 内容被卷上去的高度
        // document.onclick = function(){
        //     document.documentElement.scrollTop = 0;//到顶部
        //     console.log(document.documentElement.scrollTop);
        // }

        // 实现缓慢的滚动到顶部，有过渡效果
        document.onclick = function(){
            let top = document.documentElement.scrollTop;
            // let times = top;
           (function s(){
                if (top>0){
                    top -= 60;
                    //
                     requestAnimationFrame(s);
                    // setTimeout(s,10);
                }else{
                    top = 0;
                }
               document.documentElement.scrollTop = top;
           })();

        }
    </script>
</body>
```

