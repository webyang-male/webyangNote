## 第十八课：阿里图标、iframe、语义化标签

------

```css

	   iframe{
		   width: 500px;
		   height: 600px;
		   border:5px solid red;
	   }
<body>
		<a href="./symbol.html" target="myiframe">阿里图标</a>
		<a href="./fontclass引入方式.html" target="myiframe">阿里图标2</a>
		<iframe src="http://www.baidu.com" name="myiframe" frameborder="1" scrolling="auto"></iframe>
		
		
	</body>
```
​			<font style="color:skyblue;font-size:20px">iframe属性: </font>
​					width  height
​						frameborder="1" --- 内联框架的边框  
​											可能值: 0 没有边框线 1 有边框线
​					scrolling="auto / no / yes"
​						yes---有滚动条
​						no --- 没有滚动条
​						auto ---根据内容自动显示滚动条
​					src="" -- 链接文档的地址
​					name---iframe 的名字
​			

------

|                                                              |
| ------------------------------------------------------------ |
| 兼容:                                                        |
| -webkit- : Chrome  safari 搜狗 QQ 等等webkit内核的浏览器     |
| -moz- : 火狐                                                 |
| -o-  : Opera                                                 |
| -ms- : IE                                                    |
| 🌏	<font style="color:pink;font-size:20px"> 新增的结构标签:</font> |
|                                                              |
| 💖	header  头部                                            |
| nav 导航                                                     |
| section 定义文档中的节 一个独立的区域  对页面的内容进行分块  |
| 侧重对页面进行分块                                           |
| article  页面中独立的内容                                    |
| 可以是一篇短文,一片完整的文章,博客文章 ...                   |
| 侧重的是  表达文章  或者是帖子                               |
| aside : 侧边栏                                               |
| footer : 文档底部                                            |
|                                                              |
| 特殊结构标签:                                                |
| figure  定义独立的流内容                                     |
| <figure>                                                     |
| 这个是组合标签                                               |
| <figcaption>这是一张图片</figcaption>                        |
| <img src="icon/people%20(1).png" alt="">                     |
| <img src="icon/yifu.png" alt="">                             |
| </figure>                                                    |
| 表示是一块独立的   图片区域                                  |
| figcaption --- 定义改图片区域的标题                          |
| mark ---表示重点突出                                         |



<a href="https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.16&helptype=code" style=" text-decoration:none;"><font style="color:PeachPuff;font-size:20px">🌏阿里小图标引用网站</font></a>
<link rel="icon" type="image/x-icon" href="XXX**.ico**">