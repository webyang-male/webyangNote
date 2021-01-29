---
title: Vue01
date: 2020-12-29 10:07:57
tags: vue
categories: Vue
description: vue实例化/数据绑定与方法
---

##### vue实例创建与插值

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```vue
<div id="app">
  {{ message }}
</div>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
//Hello Vue!
```

##### vue的表达式插值

```vue
<div id="app1">
        <p>{{msg + 'hello world'}}</p>
        <p>{{num}}</p>
        <p>{{num * 10}}</p>
        <p>{{num / 10}}</p>
        <p>{{msg * 10}}</p>
        <p>{{state ? '我是真的' : '我是假的'}}</p>
        <p>{{msg.split(",")}}</p>

        <!-- 错误示例 -->
        <!-- <p>{{var a = 1}}</p> -->
        <!-- <p>{{if(1){return msg}}}</p> -->
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: 'xx,真好看',
                num: 10,
                state: 1
            }
        })
    </script>
```

##### vue的计算属性

```vue
<div id="app1">
        <p>是没有使用计算属性：<span>{{msg.split('').reverse().join('')}}</span></p>
        <p>使用计算属性：<span>{{reverseMsg}}</span></p>
        <p>{{num * 1000 / 5 + 124}}</p>
        <p>{{add}}</p>
    </div>
    
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '小尤好看',
                num: 10
            },
            computed: {
              // 计算属性的 getter
                reverseMsg: function() {
                   // `this` 指向 vm1 实例
                    return this.msg.split('').reverse().join('')
                },
                add: function() {
                    return this.num * 1000 / 5 + 10
                }
            }
        })
    </script>
```

###### [计算属性的 setter](https://cn.vuejs.org/v2/guide/computed.html#计算属性的-setter)

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter：

```js
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    <div id="app1">
        <p>{{info}}</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '风一样的男子',
                age: 18
            },
            computed: {
                info: {
                    // getter
                    get: function () {
                        return this.msg + '就是我,永远的' + this.age + '岁'
                    },
                    // setter
                    set: function () {
                        this.msg = '大赵菌';
                    }
                }
            }
        })
    </script>
</body>
---------------------------------------
 
// setter
set: function (newValue) {
     this.msg = newValue;//动态赋值msg
     this.age = newValue;//动态赋值age
}
```

![](https://source.acexy.cn/view/XatVU4r)





###### methos方法

```vue
<body>
    <div id="app1">
        <p>使用computed方法：<span>{{reverserMsg}}</span></p>
        <p>使用methods方法：<span>{{revers()}}</span></p>
    </div>
    
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg1: '风一样的女子',
                msg2: '今天天气真好'
            },
            computed: {
                reverserMsg: function() {
                    return this.msg1.split("").reverse().join("")
                }
            },
            methods: {
                revers: function() {
                    return this.msg2.split("").reverse().join("")
                }
            }
        })
    </script>
</body>
```

###### watch侦听属性

```vue
<body>
    <div id="app1">
        <p>{{name}}{{msg}}</p>
    </div>
    
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                name: 'zzq',
                msg: '像风一样自由'
            },
            watch: {
                // 在msg的值改变的时候，我们去改变name的值
                msg: function(newValue, oldValue) {
                    console.log(newValue, oldValue)
                    if (newValue) {
                        this.name = "xinxin"
                    } else {
                        this.name ="小刘"
                    }
                },
                name: function() {
                    // ...
                }
            }
        })
    </script>
</body>
```



##### 拓展

[Vue中watch、computed与methods的联系和区别]: https://juejin.cn/post/6844904086349807624#comment

