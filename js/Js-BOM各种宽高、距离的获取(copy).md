## 各种宽高、距离的获取

### 1.可视区宽高

- 窗口宽高

  `window.innerWidth    window.innerHeight`

  包含了滚动条的宽度和浏览器本身的边框宽度（低版本IE不支持）。

- 内容区宽高

  `document.documentElement.clientWidth`

  `document.documentElement.clientHeight`

  不包含滚动条等。

### 2.元素的各种宽高

- client

  `clientWidth   clientHeight `

  宽(高)+padding。

- offset

  `offsetWidth   offssetHeight`

  宽(高)+padding+border。

- scroll

  `scrollWidth / scrollHeigh`

  内容的实际高度，当内容没超出相当于client，当内容超出之后，会得到包括超出内容的实际高度，	<font style="color:skyblue;font-size:20px">**即使加了超出隐藏，也还是会得到内容所占的实际高度。** </font>

### **3.元素的各种距离**

- offset

  `offsetLeft   offsetTop`

  获取左边（上边），到定位父级的左边（上边）的距离。

- `getBoundingClientRect`

  返回一个对象，包含了元素各边到窗口的距离，返回的结构类似于：{top:100,left:20,bottom:500,right:890}。

### 4.滚动距离

- 页面滚动宽高

  `doucment.documentElement.scrollTop`

  `document.documentElement.scrollLeft`

  页面的滚动宽（高）。此属性可以赋值，能让页面滚动到指定的位置。

  设置滚动距离也可以使用`window.scrollTo()`。

- 元素滚动宽高

  `元素节点.scrollTop   元素节点.scrollLeft`

### 5.案例or作业

Js[碰撞小球]

![image-20200628203419536](D:%5Ctz%E8%AF%BE%E7%A8%8B%E8%B5%84%E6%96%99%5CTypora%E7%AC%94%E8%AE%B0%5Cjs%5Cimage-20200628203419536.png)