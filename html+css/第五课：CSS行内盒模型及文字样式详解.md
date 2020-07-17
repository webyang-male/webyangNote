## 第五课：CSS行内盒模型及文字样式详解

------

##### <font style="color:skyblue;font-size:20px">标准模式下，一个块的宽度 = width+padding(内边距)+border(边框)+margin(外边距)；</font>

##### <font style="color:skyblue;font-size:20px">怪异模式下，一个块的宽度 = width+margin(外边距)  （即怪异模式下，width包含了border以及padding）;</font>

bottom：使元素及其后代元素的底部与整行的底部对齐。
box-sizing:content-box||border-box||inherit
box-sizing:content-box;  将采用标准模式的盒子模型标准
box-sizing:border-box; 将采用怪异模式的盒子模型标准
box-sizing:inherit; 规定应从父元素继承 box-sizing 属性的值。

🌏<font style="color:yellow;font-size:20px"><a href="https://www.jianshu.com/p/99588abb1097" style=" text-decoration:none;">CSS标准盒子模型和IE怪异盒子模型</a>/来源：简书</font>

行盒模型与盒模型不同，自身是一个虚拟的盒，一行行内元素就会确定一个行盒模型。行盒模型中行盒不存在margin、border、padding属性。宽度与内容有关。高度是由不可替代元素的文字（没有margin等）可替代元素的margin box line-height值决定，盒的高度是最高盒子的top和最低盒子bottom的值。

<a href="https://blog.csdn.net/wmaoshu/article/details/53667076" style=" text-decoration:none;"><font style="color:red;font-size:20px">🌏行盒模型讲解</font></a>

------

垂直对齐不影响块级元素中内容的对齐
vertical-align：
baseline：使元素的基线与父元素的基线对齐
middle：使元素的中部与父元素的中线对齐
top：使元素及其后代元素的顶部与整行的顶部对齐

   	 文本基线：文本基线的位置是每一种字体中<font style="color:LavenderBlush;font-size:20px">英文字母x</font>的底线决定的。
               文本底线：文本底线的位置是文本框底部的位置。
               文本顶线：文本框顶部（此时图片的顶部与文本顶线对齐，其余都是底部）。
               文本中线：文本中间的位置。

 非替换元素:  是指内容可以直接在代码中写出来的内容(如文字)

 替换元素: 是指内容存储在代码文件之外,需要通过文件路径引入的内容(如图片,,音频,视频)

```html
 --> 当替换元素与非替换元素同时出现在行内模型中时, 默认以文本基线为标准进行对齐

       (文字的文本基线与图牌的底部对齐, 或者多个不同字体,不同字号的文字与图牌,音频,视频元素的对齐)

 --> 当替换元素的高度超过line-height指定的最小行框高度时,行框将被撑开, 

      (所谓的行框: 就是行内盒模型中最高部分到最低部分,以及最左边内容到最右边内容的范围)

      ** 对齐方式;  先确定最高的行内盒模型元素, 并算出该元素的文本基线, 接着将其它元素的文本基线与最高的行内盒模型元素的基线进行对齐;

        

          =>  水平方向上的对齐规则: 

                text-indent: 缩进; (可以为px, 也可以为em)

                text-align:   水平方向对齐规则( left, center, right, justify<两端对齐>)

            => 文本装饰  text-decoration

                underline: 下划线;   overline:上划线;  line-through:贯穿线;

```

<font style="color:pink;font-size:20px">👍行框的最终高度,由行内最高的那个行内盒模型决定。</font> 

使用**vertical-align**的条件

`vertical-align`是用来对齐内联级元素的。

设置为以下display属性的元素，它们都被认为属于内联级元素。

inline,

inline-block or

inline-table

inline内联元素基本上是包裹文本的标签。

😆inline-block内联块元素则如它们的名字所示：拥有内联特性的块元素。他们可以有width和height（可能是由自己的内容定义），以及padding、border和margin。

⭐内联级元素彼此紧挨着放在一行中。一旦有更多的元素被放置到当前行中，一个新的行将会在它下面创建。所有这些行有所谓的“行框”，行框中包含所有的内容。不同大小的内容意味着不同高度的行框。在下面的插图中，行框的顶部和底部都是用红线表示的。
<a href="https://www.cnblogs.com/shuiyi/p/5597187.html" style=" text-decoration:none;"><font style="color:red;font-size:20px">🌏【vertical-align详解】</font></a>

------

