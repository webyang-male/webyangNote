## 第四章：CSS盒模型概念及企业应用规则 

***

💒CSS盒模型概念及企业应用规则
			border:宽度;种类;颜色;
			double:边框实现出来其实是一条,只是分成了两个部分组成.
		两种特殊的情况:
			1.设置成1px的话,就不会有间距,因为除不尽,double也不生效.
			2.设置成2px的话,也不会有间距,因为除得尽间距自然而然也就没有

  💯 如果我们不给double边框或solid边框设置值的话,浏览器给的默认边框值为3px.

```html
	如果我们不给边框设置颜色值的话,浏览器给的默认边框色是黑色,他会默认与color里设置的字体颜色一致.

    如果我们不给边框设置种类的话会是什么样的呢?浏览器默认是没有的,也就是boder-style:none;
```
💗box moldel (w3c 规定一个浏览器如何渲染显示一个元素/标签/根据元素的种类分为块级元素盒模型   ,行内元素盒模型)

```html
  一个元素一个盒子:

  块级元素盒模型+行内元素盒模型=前段的一切

  第一步-边距清除: body {

  margin: 0;
}
  margin横向居中规则: 1、逆时针对应2- 4值居中任意像素auto任意像素auto3-2值居中任意像素auto ;

  (上下无限延伸不可auto实现垂直居中)

  content box:内容区

  文字从区域左上角开始排列(距形区的内容区)

  宽高属性:定义的是存放内容多少的区域

💙border/边框:宽度(px) 种类颜色整体设置
块级元素的border的作用是在元素的内容区外加上一个边框线
边框样式的格式为:     broder:宽度 种类 颜色;/*复合写法*/
单例型写法:
  border-width:边框宽度;
  border-style:种类;  默认是none 就是没有边框
  /*dotted圆点边框, double双边框, dashed虚线边框,solid实线边框 */
  border-color:颜色;  默认与color样式一致
 /* 颜色值: 十六进制, rgb, 关键字, transparent(使用父元素的颜色)*/

  border-top、right、 bottom、 left: 宽度(px) 种类颜色- -单独设置
  栗子:
  设置顶部边框
    border-top:宽度 种类 颜色;
    border-top-width: … …
    border-top-style: … …
    border-top-color: … …

💠padding也可以单独设置四个方向不同的值, 具体的概念与border相似
```
