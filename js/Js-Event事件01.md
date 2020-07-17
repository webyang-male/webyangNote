## Event事件

​	--2020/04/17

------

```js
<div id="box"></div>
    <script>
        // event 事件对象 ---存储和事件相关一些属性

        let box = document.getElementById("box");

        // box.onclick = function(e){
        //     console.log(e);
        //     console.log(1111);
        // };

        //  IE
        // 兼容写法
        box.onclick = function(e){
           e = e || window.event;
            console.log(e)
        }

```

### 鼠标位置

```js
<div id="box"></div>
    <script>
        let box = document.getElementById("box");
        box.onclick =function(e){

            // 事件触发的时候鼠标距离可视区的位置(左上角)
            // console.log(e.clientX);
            // console.log(e.clientY);

            // 触发事件 ,鼠标距离 文档 的位置
            // console.log(e.pageX);
            // console.log(e.pageY);

            // 鼠标距离 这个盒子 -主体 的距离
            // console.log(e.offsetX);
            // console.log(e.offsetY);

            //鼠标位置距离 用户电脑屏幕的距离
            console.log(e.screenX);
            console.log(e.screenY);

        }
    </script>
```

