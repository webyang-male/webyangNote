---
title: Vue指令二
date: 2020-12-31 08:58:02
tags: vue
categories: Vue
description: vue常见指令学习篇二
---

##### v-model指令修饰符

```vue
<body>
    <div id="app1">
        <!-- 如果要把用户输入的值类型转换为数值类型，使用.number -->
        <p>年龄：<input type="text" v-model.number="age"></p>
        <p>{{age}}</p>
        <!-- 如果要把用户输入的值首尾空格去掉使用.trim -->
        <p>用户名：<input type="text" v-model.number.trim="name"></p>
        <p>****{{name}}****</p>
        <p>密码：<input type="password" v-model.number="password"></p>

        <p>用户名：<input type="text" v-model.number.lazy="name2"></p>
        <p>{{name2}}</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                age:"",
                name:"",
                name2:"",
                password:""
            },
            watch: {
                password: function(newV, oldV) {
                    console.log(newV, typeof newV)
                }
            }
        })
    </script>
</body>
```

##### v-bind指令

v-bind指令用于响应更新HTML特性，将一个或多个attribute，或者一个组件prop动态绑定到表达式。

```vue
<body>
    <div id="app1">
        <!-- <img v-bind:src="srcUrl"> -->
        <!-- v-bind语法的简写 -->
        <img :src="srcUrl" :alt="msg">
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                srcUrl: './xx.png',
                msg: '当前图片找不到了'
            }
        })
    </script>
</body>
```

###### v-bind指令的class绑定

```vue
    <style>
        .active {
            width: 300px;
            height: 300px;
            background: yellow;
        }
    </style>
--------------------------------------------------------------------------------------------------
<body>
    <div id="app1">
        <!-- 我们可以传给v-bind:class一个对象，来进行动态的切换class -->
        <div v-bind:class="{active: isActive}">
            {{msg}}
        </div>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '古力娜扎',
                isActive: true
            }
        })
    </script>

</body>
```

###### v-bind指令的class绑定之对象绑定

```vue
    <style>
        .active1 {
            width: 300px;
            height: 300px;
            background: yellow;
        }
        .active2 {
            font-size: 50px;
        }
        .cuihua {
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="app1">
        <!-- 方法1
            可以在一个对象中传入更多的属性来动态切换多个class
            还可以跟普通的calss并存
        -->
        <div
        class="cuihua"
        v-bind:class="{active1: isActive1, active2: isActive2}">
            {{msg}}
        </div>
        <!-- 
					方法2
					绑定的数据对象可以定义在data里面而不是内联样式中 
				-->
        <div :class="goudanObj">
            {{msg}}
        </div>
    </div>
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '古力娜扎',
                isActive1: true,
                isActive2: false,
             //方法2:这里抽离出来
                goudanObj: {
                    active1: false, 
                    active2: true
                }
            }
        })
    </script>
</body>
```

###### v-bind指令的class绑定之数组绑定

```vue
 <style>
        .active1 {
            width: 300px;
            height: 300px;
            background: yellow;
        }
        .active2 {
            font-size: 50px;
        }
        .cuihua {
            text-align: center;
        }
        .errClass {
            color: red;
        }
  </style>

<body>
    <div id="app1">
        <!-- 可以把一个数组传给v-bind:class -->
        <div
            v-bind:class="[class1, class2, class3]">
            <!-- v-bind:class="classArr"> -->
            {{msg}}
        </div>
        <!-- 如果想要根据条件切换列表中的class可以使用三目表达式 -->
        <p :class="toggle ? 'active2' : 'errClass'">哈哈哈哈</p>
        <p :class="toggle ? classArr : 'errClass'">哈哈哈哈</p>
        <!-- 这样写也是可以的 -->
        <p :class="[class3, {active2: toggle}]">我的数组中的对象</p>
    </div>
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '古力娜扎',
                class1: 'active1',
                class2: 'active2',
                class3: 'cuihua',
                toggle: false,
                classArr: ['active1','active2','cuihua']
            }
        })
    </script>
</body>
```

###### v-bind指令的style绑定.

```vue
<body>
    <div id="app1">
        <div
            v-bind:style="{color: 'red', fontSize: 24 + 'px'}">
            {{msg}}
        </div>
        <div
            v-bind:style="{color: 'red', fontSize: fontSize + 'px'}">
            {{msg}}
        </div>
        <!-- 上面的方法看起来不太方便，所以我们一般情况下都是会采用对象的形式绑定的 -->
        <p :style="styleData">{{msg}}</p>
        <!-- style也是可以使用数组语法把多个样式对象应用到同一个元素中的 -->
        <p :style="[styleData, styleData2]">{{msg}}</p>
    </div>
    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: '古力娜扎',
                fontSize: 46,
                styleData: {
                    width: '300px',
                    height: '300px',
                    background: 'yellow'
                },
                styleData2: {
                    color: 'red',
                    fontSize: '60px'
                }
            }
        })
    </script>
</body>
```

##### v-for指令遍历数组

```vue
<body>
    <div id="app1">
        <li v-for="(goudan, index) in girls">
            <!-- <span>{{goudan}}</span> -->
            <span :key="index">{{index}}---{{goudan.name}}---{{goudan.sex}}---{{goudan.home}}</span>
        </li>
        <!-- 也可以使用of替代in -->
        <li v-for="(item, index) of girls">
            <span>{{item.name}}是我的{{index+1}}号女朋友</span>
        </li>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girls: [
                    {name: "郑爽", sex: "女", home: "china"},
                    {name: "佟丽娅", sex: "女", home: "china"},
                    {name: "王丽坤", sex: "女", home: "china"},
                    {name: "古力娜扎", sex: "女", home: "china"},
                    {name: "迪丽热巴", sex: "女", home: "china"}
                ]
            }
        })
    </script>
</body>
```

##### v-for指令遍历对象

```vue
<body>
    <div id="app1">
        <!-- v-for渲染元素列表的时候，默认是“就地更新”的原则 
            直接对数据对象进行新增操作，不会立即触发网页重新渲染
            只有更改了某个已经存在的属性值之后，才会进行渲染
        -->
        <li v-for="(value, key, index) in girl">
            {{index}}--{{key}}--{{value}}
        </li>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girl: {
                    name: '郑爽',
                    sex: '女',
                    age: 28
                }
            }
        })
    </script>
</body>
```

##### v-for指令之数组更新检测

Vue 将被侦听的数组的变异方法进行了包裹，所以它们也将会触发视图更新。这些方法会改变vue实例里面的数组本身。这些被包裹过的方法包括：

1. push()
2. pop()
3. shift()
4. unshift()
5. splice()
6. sort()
7. reverse()

```vue
<body>
    <!-- push、pop、shift、unshift、splice、sort、reverse会改变原数组本身 -->
    <div id="app1">
        <li v-for="(cuihua, index) in girls">
            {{cuihua}}--{{index}}
        </li>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girls: [
                    {name: '郑爽1',sex: '女',age: 12},
                    {name: '郑爽2',sex: '女',age: 14},
                    {name: '郑爽3',sex: '女',age: 15},
                    {name: '郑爽4',sex: '女',age: 17},
                    {name: '郑爽4',sex: '女',age: 19}
                ],
                arr: [3,2,5,7,9]
            }
        })
    </script>
</body>
```

###### v-for指令之过滤排序

```vue
<body>
    <div id="app1">
        <!-- filter、concat、slice不会改变原数组而是返回一个新数组 -->
        <!-- 当我们需要显示一个经过过滤或者排序之后的数组，而不是去改变原数组
        这种情况下我们就可以使用计算属性，返回一个过滤后或者是排序后的数组 -->
        <li v-for="(cuihua, index) in filterArr">
            {{cuihua}}--{{index}}
        </li>
        <hr>
        <!-- 在计算属性不适用的情况下，比如在嵌套的v-for循环中，可以使用一个方法来写 -->
        <li v-for="(cuihua, index) in filterData(girls)">
            {{cuihua}}--{{index}}
        </li>
        <hr>
        <!-- v-for指令范围：也可以接受整数，这种情况下，它会把模板重复渲染对应的次数 -->
        <li v-for="num in 5">
            hello world
        </li>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girls: [
                    {name: '郑爽1',sex: '女',age: 12},
                    {name: '郑爽2',sex: '女',age: 14},
                    {name: '郑爽3',sex: '女',age: 15},
                    {name: '郑爽4',sex: '女',age: 17},
                    {name: '郑爽4',sex: '女',age: 19}
                ],
                arr: [3,2,5,7,9]
            },
            computed: {
                filterArr: function() {
                    return this.girls.filter( function (item) {
                        return item.age > 14
                    })
                }
            },
            methods: {
                filterData: function (cuihua) {
                    return cuihua.filter(function (item) {
                        return item.age < 15
                    })
                }
            },
        })
    </script>
</body>
```



##### <font style="color:tomato;">v-for指令之注意点</font>

###### 不要在同一元素上使用 v-if 和 v-for

当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 

```vue
<body>
    <div id="app1">
        <!-- 
            不要在同一个元素上使用v-if和v-for
            当他们处在同一节点，v-for优先级比v-if高，就意味着v-if会分别重复运行在每一个v-for循环中
         -->
         <!-- 下面这个是错误写法，千万不要这样写 -->
        <li v-for="(cuihua, index) in girls" v-if="toggle">
            <span :key="index">{{cuihua}}--{{index}}</span>
        </li>
        <!-- 最好是写在外层 -->
        <ul v-if="toggle">
            <li v-for="(cuihua, index) in girls">
                <span :key="index">{{cuihua}}--{{index}}</span>
            </li>
        </ul>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girls: [
                    {name: '郑爽2',sex: '女',age: 14},
                    {name: '郑爽3',sex: '女',age: 15},
                    {name: '郑爽1',sex: '女',age: 12},
                    {name: '郑爽4',sex: '女',age: 17},
                    {name: '郑爽4',sex: '女',age: 19}
                ],
                toggle: false
            }
        })
    </script>
</body>
```



###### 必加key属性

```vue
<div id="app1">
        <!-- 
            有相同父元素的子元素必须有独特的key值，重复的key会造成渲染错误
            不要使用对象或是数组之类的非基本类型值作为key，使用字符串或者数值类型的值
         -->
        <li v-for="(cuihua, index) in girls">
            <span :key="index">{{cuihua}}--{{index}}</span>
        </li>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                girls: [
                    {name: '郑爽2',sex: '女',age: 14},
                    {name: '郑爽3',sex: '女',age: 15},
                    {name: '郑爽1',sex: '女',age: 12},
                    {name: '郑爽4',sex: '女',age: 17},
                    {name: '郑爽4',sex: '女',age: 19}
                ]
            }
        })
    </script>
</body>
```

