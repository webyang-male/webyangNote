## ajax

​		--2020/04/23

------

   **ajax  --- 全称是? Asynchronous Javascript And XML**
                                异步的      js     和   xml
                                                       		json

> ajax --- 前后端  数据交互的一种手段
> 	   ajax  好处 ---  数据拿到之后  ---- 更新局部信息

 **异步:**
            发起一个请求,要做某一件事情,
            这件事情不是立马执行的,
            等这个请求完成之后,
            再回过头来执行函数
    <font style="color:skyblue;"> 异步代码: 等你当前的同步代码执行完成之后再执行</font> 

```js
<script>
       setTimeout(function(){
            console.log("学习");
        },30);
        console.log(1111);
        console.log(2222);
    </script>
//输出结果顺序：
			1111
			2222
			学习
```

**xml  --- ?**
        *       HTML--->超文本标记语言
                *       XML ---> 标记语言 --- 一个数据交互的一种文件格式
                        *	  Json(  一种数据格式  )

```json
         let date ='{"q":"言心","g":[{"type":"sug","q":"言心日水谜底是什么"},{"type":"sug","q":"言心念什么"},{"type":"sug","q":"言必信行必果的意思"}]}';
 // 这段内容称为   json 内容
```
------

### ajax写法：

1. 创建 xhr 对象
             请求监听阶段
2. 创建请求的各种信息
                请求类型: get  /  post
                请求地址: 不能随便写
                        存在跨域问题:--- 后台操作
                请求的异步: true
3. 发布请求

```js
 // 1. 创建 xhr 对象

        let xhr = new XMLHttpRequest();
        xhr.open("get","http://localhost:3000/",true);
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send();
```

#### get发送方式

```js
 <script>
        let sendDate = {
            name:"学习",
            age:20
        };
        let xhr = new XMLHttpRequest();
        let url = "https://www.baidu.com/sugrec";

        xhr.open("get",url,true);
        url +="?";
        // 遍历
        for (let [key,value] of Object.entries(sendDate)) {
            url += `${key}=${value}`;
        }
        console.log(url);
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send();

    </script>
```

*   #### post  请求 --- url 谁不变的
    
       数据---放到  send
    
    ```javascript
     <script>
        
            let sendDate = {
                name:"学习",
                age:20
            };
            let xhr = new XMLHttpRequest();
            let url = "http://localhost:3000/";
    
            xhr.open("post",url,true);
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    
            let d ="";
            // 遍历
            for (let [key,value] of Object.entries(sendDate)) {
               d +=`${key}=${value}`;
            }
            console.log(d);
    
            xhr.send(d);
    
        </script>
    ```
    
    ### 请求阶段4
    
    ```js
    <script>
            let sendDate = {
                name:"学习",
                age:20
            };
    
            let xhr = new XMLHttpRequest();
    
            xhr.onreadystatechange = function(){
                console.log(this.readyState);
                if (this.readyState !== 4)return;
    
                // 后端返回给的数据
                // responseText 负责接收数据
                console.log(this.responseText);
            };
            xhr.open("get","http://localhost:3000/",true);
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
            xhr.send();
    
    
    
            /*
            *   请求监听阶段:
            *       0---没有创键
            *       1---请求已经建立  open
            *       2---请求已经发送  send
            *       3---请求正在后端处理
            *       4---请求已经完成,并且响应
            *
            *     关注第四个阶段
            *
            *
            * */
        </script>
    ```
    
    ### Jq的ajax
    
    ```js
    <body>
        <script src="https://cdn.bootcss.com/jquery/3.5.0/jquery.min.js"></script>
        <script>
            // console.log($);
            /*
            *   jq的  $ --- 占用一个变量名,全局变量
            *
            * */
            $.ajax({
                // 请求类型
                type:"get",
                //请求地址
                url:"http://localhost:3000/",
                date:{
                    name:"你好"
                },
                // 返回数据格式的预处理
                dateType:"json",
                // 成功的请求函数
                success:function(msg){
                    console.log(msg)
                }
            });
    
        </script>
    ```
    
    ### 案例

