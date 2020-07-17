~~~第二章html标签

😛img src ="地址”alt="图片介绍">     

😉<video scr="地址" controls ="controls"> <video> (controls给视频文件添加播放按钮)      (淘汰标签autoplay="autoplay"自动播放视频插件，占用浏览器资源默认关闭)     
     
 </video>
 </video>

🌹<audio 'scr='地址"controls="controls'></audio>      <a href="地址” target=“页面打开方式”： ‘_black’新页面打开; _self 默认打开方式,当前页打开
</a>默认情况下原网页跳转     <p></p>引用大段文字📱<blockquote> <blockquote>引用标准文字格式      <pre></pre>保留引用前原文字格式     

 <br/>换行标签单标签标签后一句进行换行      
🏆
<hr/>单标签水平线标签前一句下面出现水平线(单标签后/可写可不写效果一样)
~~~

![image-20200204003834792](D:%5C%E7%AC%94%E8%AE%B0%E8%B5%84%E6%96%99%EF%BC%88%E8%8B%B1%E8%AF%AD,web%EF%BC%89%5CTypora%E7%AC%94%E8%AE%B0%5Cimage-20200204003834792.png)
**概念:独占一行的标签为块级元素**
**可以和别的元素，和同个标签占一排的叫行内元素**

**块级元素独占一行(可以嵌套任何元素)div h ul	ol	li		dl	dd	dt	p......**
**行内元素并列一行(只 能嵌套行内元素)文字元素都是行内**
**span b strong em i br hr img vedio	audio......特殊情况**
**p标签(不可以嵌套块级) p div...都不行**

## 🍀[块级元素和行内元素](https://www.cnblogs.com/stfei/p/9084915.html)

##        <img src="D:%5C%E7%AC%94%E8%AE%B0%E8%B5%84%E6%96%99%EF%BC%88%E8%8B%B1%E8%AF%AD,web%EF%BC%89%5CTypora%E7%AC%94%E8%AE%B0%5Cx.png" style="zoom: 67%;" />

​    

## 🌴1.标签分为两种等级：

### 　　1-1行内元素	

###         1-2块级元素

## 2.行内元素和块级元素的区别：

行内元素：　　

- 与其他行内元素并排
- 不能设置宽高，默认的宽度就是文字的宽度

块级元素：

- 霸占一行，不能与其他任何元素并列。
- 能接受宽高，如果不设置宽度，那么宽度将默认变为父级的100%。

## 🏆3.块级元素和行内元素的分类：

　　在HTML的角度来讲，标签分为：

　　　　文本级标签：p , span , a , b , i , u , em

　　　　容器级标签：div , h系列 , li , dt ,dd

　　　　p：里面只能放文字和图片和表单元素，p里面不能放h和ul，也不能放p。

　　从CSS的角度讲，CSS的分类和上面的很像，就p不一样：

　　　　行内元素：除了p之外，所有的文本级标签，都是行内元素。p是个文本级标签，但是是个块级元素。

　　　　块级元素：所有的容器级标签，都是块级元素，以及p标签。



## 4.块级元素和行内元素的相互转换：

　　我们可以通过display属性将块级元素(比如div)和行内元素进行相互转换。

　　display：inline;

　　那么这个标签将变为行内元素，即：

　　　　1，此时这个div将不能设置宽度和高度了。

　　　　2，此时这个div可以和其他行内元素并排了。

　　同样的到了我们也可以用display将行内元素(比如span)转行成块级元素。

　　🌷display：block；

　　那么这个span标签将变为块级标签，即：

　　　　1，此时这个span能够设置宽度，高度。

　　　　2，此时这个span必须独占一行，其他元素无法与之并排。

　　　　3，如果不设置宽度，将占满父级。
✎tip:

- a标签(可以嵌套任何元素)

- 快捷键tap缩进
  shift+tap反缩进
- ctrl+?快捷注释

- ctrl+E自动补齐
  ctrl+shift+U所有文本首字母大写
- ctrl+z撤销     ctrl+y反撤销
  ctrl+b跳转到代码展示效果

🎯[详解块级元素等](https://blog.csdn.net/qq_32925031/article/details/89763327)