## 面向对象之继承

​				---2020/04/30

------

### **继承**

​            儿子 继承 爸爸的所有东西
  		  	   儿子 可以额外的添加新的属性,并且不影响父亲的

```js
<script>
        
        function A(){}
        A.prototype.x = 233;
        // A.prototype.z = 11111;

        function B(){}
        B.prototype.y = 888;

        B.prototype = new A();
        B.prototype.z = 11111;

        var b = new B();
        var b2 = new B();

        // console.log(b.x);

        var a = new A();
        // console.log(a.x);
       
        // console.log(b.z);
        // console.log(b2.z);
        // console.log(a.z);

        // 不想 A有 z属性, 只想 B 有z属性
        console.log(a.z);//undefined
        console.log(b.z);//11111
        console.log(b.x);//233


    </script>
```

```js
<script>
        /*
            var a ={
                aa:11,
            bb:22
        };

        var b = a;

        console.log(b.aa);
        console.log(b.bb);

        b.cc = 33;

        console.log(b.cc);
        console.log(a.cc);
        */

        var a ={
            aa:11,
            bb:22
        };

        var b = {};

        for (let aKey in a) {
            b[aKey] =a[aKey]
        }
        b.cc=44;
        console.log(b.aa);//11
        console.log(b.bb);//22
        console.log(a.cc);//undefined
        console.log(b.cc);//44


    </script
```

### 面向对象继承

```js
<script>
        // 父
        function Person(n,a){
            this.name = n;
            this.age = a;
        }

        Person.prototype.msg = function(){
            console.log(`姓名: ${this.name},年龄: ${this.age}`)
        };

        // 子

        function Student(n,a,id){
            Person.call(this,n,a);
            this.id = id;
        }

        function F(){}  // 中间人
        F.prototype = Person.prototype;  //
        Student.prototype = new F();

        Student.prototype.constructor = Student;
        Student.prototype.school = "潭州大学";
        Student.prototype.card =function(){
            console.log(this.id)
        };

        var who = new Student("who",15,20200430);
        var xiaomei = new Student("小妹",16,20200429);

        who.msg();
        xiaomei.msg();
        console.log(who)

    </script>
```

------

### ES6面向对象

```js
<script>
      
        class Teacher{
            // 固定写法 -- 私有属性
            constructor(n,a) {
                this.name = n;
                this.age = a;
            }

            // es6  只允许原型上边有方法(函数) ,不能有属性
            say(){
                console.log(`${this.name},${this.age}`);
            }

        }

        let yanxin = new Teacher("言心",18);
        let af = new Teacher("阿飞",38);

        console.log(yanxin);
        console.log(af);
        yanxin.say();
    </script>
```

------

### ES6继承

```js
 <script>
        // 父类
         class Preson{
             constructor(n,a) {
                 this.name = n;
                 this.age = a;
             }
             msg(){
                 console.log(`${this.name},${this.age}`);
             }
         }
         // 子类
        class Teacher extends Preson{
             constructor(n,a,i) {
                 super(n,a); // 私有属性的继承

                 this.id = i; // 私有属性的新增
             }

             Id(){
                 console.log(this.id);
             }
        }


        let s = new Teacher("言心",18,88);
        console.log(s);
        s.Id();
        s.msg();


    </script>
```

