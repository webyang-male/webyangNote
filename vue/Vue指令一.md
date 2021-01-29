---
title: Vue指令一
date: 2020-12-29 16:43:54
tags: vue
categories: Vue
description: vue常见指令
---

##### 一、指令简介

指令是带有v-前缀的特殊特性。指令特性的值预期是单个JavaScript表达式。
	   <font style="color:skyblue;">指令的职责是：当表达式的值改变时，将其产生的连带影响，响应式的作用于DOM。</font>

##### 二、指令学习

###### 1、v-if指令

if指令可以完全根据表达式的值在DOM中生成或移除一个元素。如果v-if表达式赋值为false，那么对应的元素就会从DOM中移除；否则，对应元素的一个克隆将被重新插入DOM中。记住，这个是直接决定是否在网页进行渲染，而不是元素是否显示。

###### 2、v-else指令

v-else就是JavaScript中else的意思，它必须跟着v-if，充当else功能。

###### 3、v-else-if指令

v-else-if，顾名思义，充当v-if的“else-if块”，可以连续使用。类似于v-else，v-else-if也必须紧跟在带v-if或者v-else-if的元素之后。
**注意：这些条件判断只会有一个生效**

###### 4、v-show指令

v-show指令是根据表达式的值来显示或者隐藏HTML元素。当v-show赋值为false时，元素将被隐藏。查看DOM时，会发现元素上多了一个内联样式`style="display:none"`。

###### 5、v-if和v-show指令详解

在切换v-if模块时，vue.js有一个局部编译/卸载过程，因为v-if中的模板可能包括数据绑定或子组件。v-if是真实的条件渲染，因为它会确保条件块在切换时合适的销毁与重建条件块内的事件监听器和子组件。

v-if是惰性的 一一 如果初始渲染时条件为假，则什么也不做，在条件第一次变为真时才开始局部编译（编译会被缓存起来）。

相比之下，v-show简单的多 一一 元素始终被编译并保留，只是简单地基于切换。

一般来说，v-if有更高的切换消耗，而v-show有更高的初始渲染消耗。

因此，如果需要频繁的切换，则使用v-show较好；如果在运行时条件改变较少，则使用v-if较好。

###### 6、v-model指令

v-model指令用来在input、select、text、checkbox、radio等表单控件元素上创建双向数据绑定。根据控件类型v-model自动选取正确的方法更新元素。尽管有点神奇，但是v-model不过是语法糖，在用户输入事件中更新数据。

**v-model指令详解**

v-model会忽略所有表单元素的value、checked、selected特性的初始值而总是将Vue实例的数据作为数据来源。你应该通过JavaScript在组件的data选项中声明初始值。

v-model在内部为不同的输入元素使用不同的属性并抛出不同的事件：

1. text 和 textarea元素使用 value 属性和 input 事件
2. checkbox 和 radio 使用 checked 属性和 change 事件
3. select 字段将 value 作为 prop 并将change 作为事件

v-model指令修饰符：

1. number：如果想自动将用户的输入值转为数值类型，可以给 v-model添加number修饰符
2. trim：如果要自动过滤用户输入的首尾空白字符，可以给 v-model添加trim修饰符

------

##### v-if

```vue
<body>
    <div id="app1">
        <p v-if="toggle">{{msg}}</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
              //值为false时,没有dom元素,不仅仅是视觉上的"消失"
                toggle: false,//为true显示
                msg: "今天心情真好啊"
            }
        })
    </script>
</body>
```

##### v-else

```vue
<body>
    <div id="app1">
        <p v-if="toggle">{{msg}}</p>
        <p v-else>这里是else的内容</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
              	//toggle值为false,只展示v-else的p标签内容
              	//toggle值为true,只展示v-if(条件成立)的p标签内容
                toggle: false,
                msg: "今天心情真好啊"
            }
        })
    </script>
</body>
```

##### v-else-if

```vue
<body>
    <div id="app1">
        <p v-if="type === 1">1</p>
        <p v-else-if="type === 2">2</p>
        <p v-else-if="type === 3">3</p>
        <p v-else-if="type === 4">4</p>
        <p v-else-if="type === 5">5</p>
        <p v-else>6</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                type: 1//这里输出1
            }
        })
    </script>

</body>

```

##### v-show

```vue
<body>
    <div id="app1">
        <p v-show="toggle">{{msg}}</p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                toggle: true,
                msg: "今天心情真好啊"
            }
        })
    </script>
</body>
```

##### v-model

```vue
<body>
    <div id="app1">
        <p>姓名：<input type="text" v-model="name"></p>
        <p>{{state ? "zzq" : name}}</p>
        <p>姓名：<input type="text" v-model="name"></p>
        <p>{{name}}</p>
        <p>
            性别：
            <input type="radio" id="man" value="男" v-model="sex">
            <label for="man">男</label>
            <input type="radio" id="woman" value="女" v-model="sex">
            <label for="woman">女</label>
        </p>
        <p>
            你的职业是？
            <select v-model="work">
                <option value="程序员">程序员</option>
                <option value="医生">医生</option>
                <option value="老师">老师</option>
                <option value="律师">律师</option>
            </select>
        </p>
        <p>
            你喜欢二哈吗？(单个复选框绑定布尔值)
            <input type="checkbox" v-model="dog">
        </p>
        <p>
            你的女朋友是？(多个复选框绑定到同一个数组)
            <input type="checkbox" v-model="girl" value="郑爽">郑爽
            <input type="checkbox" v-model="girl" value="王丽坤">王丽坤
            <input type="checkbox" v-model="girl" value="佟丽娅">佟丽娅
            <input type="checkbox" v-model="girl" value="古力娜扎">古力娜扎
            <input type="checkbox" v-model="girl" value="迪丽热巴">迪丽热巴
        </p>
        <p>
            你对自己的评价？
            <textarea v-model="desc"></textarea>
            <span>{{desc}}</span>
            <textarea>{{desc}}</textarea>
        </p>
    </div>

    <script>
        let vm1 = new Vue({
            el: "#app1",
            data: {
                name: "",
                state: true,
                sex: '',
                work: '程序员',
                dog: true,
                girl: [],
                desc: "goodJob"
            }
        })
    </script>
</body>
```

