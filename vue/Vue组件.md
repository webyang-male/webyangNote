---
title: Vue组件
date: 2021-01-03 11:58:23
tags: vue
categories: Vue
description: vue组件
---

#### vue组件注册

```vue
<body>
    <div id="app1">
        {{msg}}
        <!-- 2、组件的使用 -->
        <item-one></item-one>
        <item-one></item-one>
        <item-one></item-one>
        <item-one/>
    </div>
    <script>
        // 1、组件注册的语法:Vue.component('name', {})第一个参数是组件名称，第二个参数是模板
        // 2、组件名定义时可以使用驼峰，但是使用时一定要用-连接
        Vue.component('item-one', {
            template: '<li>这是第一个组件</li>'
        })

        let vm1 = new Vue({
            el: "#app1",
            data: {
                msg: "像风一样自由"
            }
        })
    </script>
</body>
```

