## 表单键盘事件

​		----2020/04/22

------

### onkeydown  onkeypress  onkeyup

```js
// 键盘按下就会触发
         document.onkeydown = function(){
          console.log("我按下了一个键键")
      };
```

```js
// onkeypress----能键入内容的键才能触发,能输入字符,回车键也是可以的
        document.onkeypress = function(e){
            console.log(e.key);
            console.log(e.keyCode);
        };


     //   阻止某个键的功能发生
        document.onkeydown =function(e){
            if (e.keyCode === 116 ){
                e.preventDefault();
            }
        };

      //  键盘
        document.onkeyup = function(e){
            console.log("我抬起来了");
        }

```

### 表单事件

```js
 	<input type="text" id = "ipt">
        
		// 通过js焦点的操作:
        ipt.onfocus=function(){
            console.log("焦点")
        };
        // 失去焦点事件
        ipt.onblur = function(){
            console.log("失去焦点")
        }

 		//通过js来获得焦点
        // ipt.focus(); ipt.blur()

        // 输入监听事件: 只要按键输入内容就会触发
        ipt.oninput = function () {
            if (this.value.length >= 6){
                // 失去焦点
                this.blur();
            }
        }

```

### Change事件

```js
<body>
    <input type="text" id="ipt">

    <input type="radio" name="sex" id="radio1">男
    <input type="radio" name="sex" id="radio2">女

    <input type="checkbox" name="ai" id="check1">小a
    <input type="checkbox" name="ai" id="check2">小b
    <input type="checkbox" name="ai" id="check3">小c

    <select name="sele" id="select">
        <option value="aaa">星期一</option>
        <option value="aaa">星期二</option>
        <option value="aaa">星期三</option>
        <option value="aaa">星期四</option>
        <option value="aaa">星期五</option>
    </select>
    <script>
        let ipt = document.getElementById("ipt");
        // text : 失去焦点的时候,表单的内容和获得焦点的时候,内容不一样就会触发
        // ipt.onchange = function(){
        //     console.log("触发了change事件")
        // };

        // radio 点选自己 导致自己状态发生改变,触发
        // let radio = document.querySelectorAll("input[type=radio]");
        // radio[0].onchange = function(){
        //     console.log("触发了change事件22222")
        // };

        // checkbox : 用户的操作导致自己的选中状态发生改变触发
        let checkbox = document.querySelectorAll("input[type=checkbox]");
        checkbox[0].onchange = function(){
            console.log("触发了change事件333333333333")
        };

        // select ******** onchange  在下拉框中 ******
        let select = document.getElementById("select");

        select.onchange =function(){
            console.log("触发了change事件select")
        }

    </script>
</body>
```

### form

> reset() 方法可以重置表单所有元素的值 (与点击 Reset 按钮效果一致）。

```js
<body>
    <form action="" method="get">
        <input type="text" name="user"><br>
        <input type="password" name="pwd"><br>
        <input type="submit"><br>
        <input type="reset"><br>
    </form>
    <input type="button" value="form外边的普通按钮">
    <script>
        let form = document.querySelector("form");
        let reset = document.querySelector("input[type=button]");

        reset.onclick = function(){
            // form.reset();
            form.submit();
        }

    </script>
</body>
```

