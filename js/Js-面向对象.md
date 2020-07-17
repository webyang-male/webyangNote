## 面向对象

​		---2020/04/29

------

  面向对象 -- 功能 { 传入参数 }
            特点: 封装,继承,多态
        				狗吃香肠 -- 狗 吃( 细节 ) 香肠
        狗.吃(香肠)

小a -- 人 类
            年龄 ,性别,身高,体重  -- 属性
            走,跑,说话,---方法


        构造函数:
            new 函数()
            通过new 去执行的这个函数-构造函数 (类)
    
            特点:
                1.函数内部会创建一个全新的对象,函数内部的this指向这个对象
                2.函数默认返回值不再是 undefined 而是这个对象
```js
<script>
        //  47个  同学
        function People(name,age){
            this.name = name;
            this.age = age;
        }

        // People.prototype.eat = function(){
        //     console.log(`我是${this.name},今天吃了蛋糕`);
        //     console.log(this)
        // };

        People.prototype = {
            constructor:People,
            teacher:"言心",
            eat : function(){
                console.log(`我是${this.name},今天吃了蛋糕`);
                console.log(this)
            }
        };

        let who =new People("who am i",16);
        let xiaomei =new People("小妹",16);
        console.log(who.eat());
        console.log(xiaomei.eat());

        console.log(who.eat === xiaomei.eat); // true

        //  __proto__  -- 原型

        // 所有的 函数里边都有一个 属性 protoType  ---- 对象的__proto__
        // People.prototype -- 原型

        console.log(who.__proto__ === People.prototype); // true

        // 对象是怎么产生的 --- 由构造函数产生的



        // var a = function (){};
        // console.dir(a.constructor)

        console.dir(document.body);

        Object.prototype.aaa = 99999;  // 禁止
        console.log(who.aaa);


    </script>
```

