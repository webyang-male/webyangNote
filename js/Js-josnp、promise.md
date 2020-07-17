## Josnp和Promise

​				---2020/04/27

------

### 简单回顾ajax

```json
<script>
        let senDate={
            name:"好好学习",
            age:15,
            job:"学生"
        };

        // 创建xhr 对象
        let xhr = new XMLHttpRequest();
        // 创建请求对应的各种信息: 请求类型,请求地址,是否异步
        let url = "http://localhost:3000/";
        url+="/";
        for (let [key,value] of Object.entries(senDate)) {
            url += `${key}=${value}`;
        }
        console.log(url);
        xhr.open(
            "get",
            url,
            true
        );
        // 发送请求
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send();



    </script>
```

### 	ajax的post

```js
<script>
    let senDate={
        name:"zzy",
        age:15,
        job:"学生"
    };

    // 创建xhr 对象
    let xhr = new XMLHttpRequest();
    // 创建请求对应的各种信息: 请求类型,请求地址,是否异步
    let url = "http://localhost:3000/";
    xhr.open("post", url,true);
    // 发送请求
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
    let da = "";
    for (let [key,value] of Object.entries(senDate)) {
        da += `${key}=${value}`;
    }
    console.log(da);
    xhr.send(da);
</script>
```

------

### Jq的ajax

```json
 <script src="https://cdn.bootcss.com/jquery/3.5.0/jquery.min.js"></script>
    <script>
        $.ajax({
            // 请求类型
            type:"get",
            // 请求地址
            url:"http://localhost:3000/",
            // 需要发送的数据
            date:{ name:"雷哥",age:12},
            // 返回数据格式预处理
            dateType :"json",

            // 成功的回调函数,第一个形参代表返回的数据
            success : function(msg){
                console.log(JSON.parse(msg).g[1].q);
                // 进行  dom  .....
            }
        });

    </script>
```

------

### 跨域问题

```js
  <script>
        // 浏览器阻止跨域请求? --- 病毒
        /*
            解决跨域问题的方案有两种:
                1. 一种是在对应的地址的后端设置允许你的html地址的请求
                2. 另外一种是使用 jsonp 的形式请求数据
        */
       // 类似这种形式得到: 函数名(json数据)  --- jsonp

        function yanxin(obj){
            console.log(obj)
        }
        yanxin();
    </script>


    <script src="https://www.baidu.com/sugrec?pre=1&p=3&ie=utf-8&json=1&prod=pc&from=pc_web&sugsid=1432,31326,21119,31424,31342,30910,31270,30823,31163&wd=%E8%A8%80%E5%BF%83&req=2&bs=%E8%A8%80%E5%BF%83&pbs=%E8%A8%80%E5%BF%83&csor=2&cb=yanxin">
    </script>
```

### 案例

**josnp**百度搜索

