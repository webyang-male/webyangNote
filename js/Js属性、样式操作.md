## 属性、样式操作

一个对象初始就有很多**属性**或**方法**，这些属性和方法是JS原本就赋予这个对象的，我们可以利用他们来达到我们希望的效果。同时，我也可以给予对象添加一些它初始没有的属性或方法。

我们所说的操作可分为 **读操作** 和 **写操作** 。

### 1.标签属性

- 合法标签属性

……（className的特殊性）

- 自定义标签属性

设置`setAttribute` 获取`getAttribute` 移除`removeAttribute` 。

### 2.自定义属性

注意自定义属性和自定义标签属性的区别，注意自定义属性和变量的区别。

不推荐为某个系统对象或者节点对象添加自定义属性……

### 3.操作css样式

js之所以能改变元素的样式，基本原因就是js改变了他的css。

改变元素样式的方式：外部样式表、内部样式表、行内样式。所以我们要JS修改元素的css样式，只能通过这三个方式。

> **外部样式表**，前端的JS不能操作外部文件，所以无法修改这个；
>
> **内部样式表**，JS可以操作HTML页面里面任何元素，所以我们可以尝试操作style标签；
>
> **行内样式**，JS操作元素样式最常用的方式。`元素.style.属性` `元素.style.cssText`。

个别属性会有兼容性的写法，需要注意，可以使用class名字来代替。

*<font style="color:red;font-size:20px"> 注意：涉及到多个属性的操作是，不如添加className来的方便。</font>可以使用.style的形式获取，但是只能获取行内样式，具体获取最终展示样式的方式后面会讲到。*

### 4.获取元素的显示样式

.style.属性  的形式只可以获取行内样式，当我们要获取一个元素的显示样式时，需要使用DOM提供的额外API实现：

```js
var oWrap = document.getELementById("wrap");

var styleObj = getComputedStyle(oWrap);

var width = styleObj.width;//获取宽度样式
var height = styleObj.height;//获取高度样式
//……
```

## <font style="color:skyblue;font-size:20px">getComputedStyle: </font>

#### 一、getComputedStyle() 用法

```
document.defaultView.getComputedStyle(element[,pseudo-element]);  
或者
window.getComputedStyle(element[,pseudo-element]);
```

首先是有两个参数，元素和伪类。第二个参数不是必须的，当不查询伪类元素的时候可以忽略或者传入 null。

使用示例：

```js
let my_div = document.getElementById("myDiv");
let style = window.getComputedStyle(my_div, null);
```

关于 defaultView 引用一下 MDN 对于 defaultView 的描述:

**defaultView**

在许多在线的演示代码中, getComputedStyle 是通过 document.defaultView 对象来调用的。 大部分情况下，这是不需要的， 因为可以直接通过 window 对象调用。但有一种情况，你必需要使用 defaultView, 那是在 Firefox 3.6 上访问子框架内的样式 (iframe)。

而且除了在 IE8 浏览器中 **document.defaultView === window** 返回的是 false 外，其他的浏览器（包括 IE9 ）返回的都是 true。所以后面直接使用 window 就好，不用在输入那么长的代码了。

------

#### 二、返回值

**getComputedStyle** 返回的对象是 **CSSStyleDeclaration** 类型的对象。取数据的时候可以直接按照属性的取法去取数据，例如 **style.backgroundColor**。需要注意的是，返回的对象的键名是 **css** 的驼峰式写法，**background-color -> backgroundColor**。

需要注意的是 **float** 属性，根据 《JavaScript 高级程序》所描述的情况 ，float 是 JS 的保留关键字。根据 DOM2 级的规范，取元素的 float 的时候应该使用 cssFloat。其实 chrome 和 Firefox 是支持 float 属性的，也就是说可以直接使用。

```js
var float_property = window.getComputedStyle.style; // chrome 和 Firefox支持
```

而在任何版本的 IE 中都不能这样使用，并且在 IE 8 中仅支持 **styleFloat** ，这个下面的兼容性问题中谈到。

------

#### 三、和 style 的异同

**getComputedStyle** 和 **element.style** 的相同点就是二者返回的都是 CSSStyleDeclaration 对象，取相应属性值得时候都是采用的 CSS 驼峰式写法，均需要注意 float 属性。

**而不同点就是：**

- `element.style` 读取的只是元素的**内联样式**，即写在元素的 style 属性上的样式；而 `getComputedStyle` 读取的样式是最终样式，包括了**内联样式**、**嵌入样式**和**外部样式**。
- `element.style` 既支持读也支持写，我们通过 `element.style` 即可改写元素的样式。而 `getComputedStyle` 仅支持读并不支持写入。我们可以通过使用 `getComputedStyle` 读取样式，通过 `element.style` 修改样式

**我们可以通过使用 \**getComputedStyle\** 读取样式，通过 \**element.style\** 修改样式。**

