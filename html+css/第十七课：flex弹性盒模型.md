## 第十七课：flex弹性盒模型

------

弹性盒模型
				1. flex --- 灵活性
				2. 采用flex布局的元素---flex容器
				3.在容器里边的所有的子元素自动成为这个容器的成员--flex项目
				4.设置了flex布局以后, 子元素的 float clear 和 vertical-align属性失效
				
				

```html
			flex:  父元素 控制 子元素的布局
				使用场景: float inline-block  position  结构布局中的错乱问题,使用在移动端
				核心理念:父元素 控制 子元素的布局操作
				 
			******布局:方向(  元素的排列方向 , 换行的方向  )
			
			flex容器本身的属性:   6 
			
			****主轴: 元素的排列方向( 默认向右 ) : 4个方向
					flex-direction:
						row : 默认值---向右
						row-reverse  ---向左
						column ---向下
						column-reverse -- 向上
			****交叉轴 : 换行方向 (1个不换行(默认值)+2个值)
					flex-wrap:
						nowrap --- 不换行
						wrap --- ( 要么 向下 , 要么 向右  )
						wrap-reverse ---( 要么 向上,  要么 向左  )
			
			flex-flow: flex-direction flex-wrap;
			
			总结: 坐标轴(方向): 只有确定两个轴,后续的布局才会有意义,主轴的开始位置
			
			元素在主轴上的对齐方式:[详见笔记文件夹图片]
				justify-content:
					flex-start
					flex-end
					center
					space-around :每个 项目  两侧的间隔相等
					space-between : 两端对齐  每个项目之间的间隔相等

			交叉轴对齐方式:
				align-items: 每行内部元素的排列
					flex-start
					flex-end
					center
					baseline
				align-content: 换行的这几行元素的对齐方式(每一行看成一个整体)
								只有一行不起作用的
					flex-start
					flex-end
					center
					space-around : 每个行两侧间隔相等
					space-between : 两端对齐 每个行之间的 间隔相等
			
			项目 : 属性  --- 6	
				子元素自己微调
				order : 排序
					默认值 : 0 
					1 2 3 ......
					可以为负值
				order:值越小,排在最前边
				
				flex-grow: 分配容器的多余空间 ---伸
					默认值 : 0 
					1 --- 每个项目都设置 --- 雨露均沾(平均分配)
					单个给每个项目设置---放大
					2 3 4 ...
				flex-shrink:
					1 默认值 收缩自己
					0 不收缩
					2 3 4 ... 单个设置收缩
					
					压缩比例: 元素不换行 ---被挤压
							 谁大谁压缩的厉害
					
				flex-basis:主轴上项目的默认长度
					默认值: auto -- 内容决定
					200px ...
					
				flex: 以上三个属性的复合写法: 
					flex:flex-grow flex-shrink flex-basis ;
							0          1            auto
					***flex: 1;
				
				了解 :
					align-self:允许单个项目与其他的项目又不一样的对齐方式,
					可以覆盖容器的 align-items 属性
					flex-start
					flex-end
					center
					baseline
```