---
title: Vue指令三
date: 2021-01-02 14:39:09
tags: vue
categories: Vue
description: vue指令学习篇三
---

#### v-on指令

可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

**缩写**：`@`

##### 代码绑定

```vue
<body>
    <div id="app1">
        <!-- 当num > 4的时候，就让列表展示，否则就不展示列表 -->
        <button @click="num++">点击增加</button><button v-on:click="num--">点击减少</button>
        <p>点击次数为:{{num}}</p>
        <ul v-if="toggle">
            <li v-for="(cuihua, index) in girls">
                <span :key="index">{{cuihua.name}}</span>
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
                num: 0
            },
            computed: {
                toggle: function () {
                    return this.num > 4 ? true : false
                }
            },
        })
    </script>
</body>
```

##### 事件绑定

```vue
<body>
    <div id="app1">
        <!-- 当num > 4的时候，就让列表展示，否则就不展示列表 -->
        <button @click="add">点击增加</button><button v-on:click="num--">点击减少</button>
        <p>点击次数为:{{num}}</p>
        <ul v-if="toggle">
            <li v-for="(cuihua, index) in girls">
                <span :key="index">{{cuihua.name}}</span>
            </li>
        </ul>
        <hr>
        <!-- 可以直接调用并且传递参数 -->
        <button @click="print('msg')">点击打印</button>
        <hr>
        <input type="text" v-model="text">
        <button @click="print(text)">点击打印动态参数</button>
        <hr>
        <!-- 如果需要处理原生DOM事件，可以使用特殊变量$event把它传到方法里面去 -->
        <button @click="getDom($event,text)">点击查看原生事件</button>
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
                num: 0,
                text: ''
            },
            computed: {
                toggle: function () {
                    return this.num > 4 ? true : false
                }
            },
            methods: {
                add: function () {
                    this.num ++
                },
                print(text) {
                    console.log(text)
                },
                getDom(event, t) {
                    console.log(event, t)
                }
            },
        })
    </script>
</body>
```

##### 事件修饰符

```vue
<body>
    <div id="app1">
        <!-- 
            .stop 阻止事件冒泡
            .prevent 阻止默认事件

            .self 当事件在元素本身触发时才触发事件
            .capture 添加事件侦听器，使用事件捕获模式
            .once 事件只触发一次
         -->
         <!-- <input v-model="text" v-on:keyup.enter="doThis">
         <input v-model="text" @keyup.enter="doThis">
         <input v-model="text" @keyup.13="doThis">
         <input v-model="text" @keydown.tab="doThis"> -->
         <!-- <input v-model="text" @keydown.65="doThis"> -->
         <!-- <input v-model="text" @keydown.delete="doThis"> -->
         <!-- <input v-model="text" @keydown.esc="doThis"> -->
         <!-- <input v-model="text" @keydown.up="doThis">
         <input v-model="text" @keydown.down="doThis">
         <input v-model="text" @keydown.left="doThis">
         <input v-model="text" @keydown.right="doThis"> -->
         <!-- 你希望按下ctrl键的时候触发,即使你按下ctrl和alt也会触发的 -->
         <!-- <input v-model="text">
        <button @click.ctrl="doThis">点击</button> -->

        <!-- 有且只有ctrl被按下的时候点击才会触发事件 -->
        <!-- <input v-model="text">
            <button @click.ctrl.exact="doThis">点击</button> -->

        <!-- 没有任何系统按键被按下时才触发 -->
        <input v-model="text">
        <button @click.exact="doThis">点击</button>
        
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                text: ''
            },
            methods: {
                doThis() {
                    console.log(this.text)
                }
            },
        })
    </script>
</body>
```

##### 多事件绑定

```vue
<body>
    <div id="app1">
        <!-- 如果绑定相同的事件，则触发第一个事件，不会这样写 -->
        <!-- <button @click="doThis" @click="show">点击</button> -->
        <!-- <button @click="show" @click="doThis">点击</button> -->

        <!-- 所以我们这里说的是单击和双击两个事件同事绑定 -->
        <button @click="doThis" @dblclick="show">点击</button>
    </div>

    <script>--
        let vm1 = new Vue({
            el: "#app1",
            data: {},
            methods: {
                doThis() {
                    console.log('hello 1 单击')
                },
                show() {
                    console.log('show 1 双击')
                }
            },
        })
    </script>
</body>
```

##### once指令(不常用)

如果我们只想在网页加载时，只渲染一次数据，后期即使是data种的数据变化了，我们也不要再次进行渲染，那么我们可以使用v-once指令。

```vue
<body>
    <div id="app1">
        <p>{{msg}}</p>
        <!-- v-once只渲染元素或者组件一次 -->
        <p v-once>{{msg}}</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: "hello"
            }
        })
    </script>
</body>
```

##### v-html(不常用)

双大括号把数据解释为普通文本，而非HTML代码。所以在需要输出真正的HTML时，我们就可以使用v-html

```vue
<body>
    <div id="app1">
        <p>{{span}}</p>
        <p v-html="span"></p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: "hello",
                span: "<span>这是v-html(不常用)用法</span>"
            }
        })
    </script>
</body>
```

