## CSS部分面试题

### 1.移动端如何解决1px问题
用媒体查询根据dpr用“伪元素+transform”对边框进行缩放

用JS根据屏幕尺寸和dpr精确地设置不同屏幕所应有的rem基准值和initial-scale缩放值。

参考： [移动端高清适配方案（解决图片模糊问题、1px细线问题）](http://www.cnblogs.com/superlizhao/p/8729190.html)

### 2. 介绍flex布局
Flex是Flexible Box的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为Flex布局。

参考： [Flex布局语法教程](http://www.runoob.com/w3cnote/flex-grammar.html), [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Aligning_Items_in_a_Flex_Container)

### 3.水平居中的方法
1. 元素为行内元素，设置父元素text-align:center

2. 如果元素宽度固定，可以设置左右margin为auto;

3. 如果元素宽度固定,通过使用绝对定位，以及设置元素margin-left为其宽度的一半；

4. 如果元素为绝对定位，设置父元素position为relative，元素设left:0;right:0;margin:auto;

5. 使用flex-box布局，指定justify-content属性为center

6. display设置为tabel-ceil,text-aligin:center

参考：[水平居中](https://blog.csdn.net/dengdongxia/article/details/80297116)

### 4.垂直居中的方法
1. 将显示方式设置为表格，display:table-cell,同时设置vertial-align：middle

2. 使用flex布局，设置为align-item：center

3. 绝对定位中设置bottom:0,top:0,并设置margin:auto

4. 绝对定位中固定高度时设置top:50%，margin-top值为高度一半的负值

5. 文本垂直居中设置line-height为height值

6. display设置为tabel-ceil,vertical-aligin:center

参考：[垂直居中](https://www.cnblogs.com/hutuzhu/p/4450850.html)

### 5. 简要介绍一下CSS3的新特性
1. 在布局方面新增了flex布局；
2. 在选择器方面新增了例如:first-of-type,nth-child等选择器；
3. 在盒模型方面添加了box-sizing来改变盒模型，
4. 在动画方面增加了animation、2d变换、3d变换等。在颜色方面添加透明、rgba等，
5. 在字体方面允许嵌入字体和设置字体阴影，同时当然也有盒子的阴影，
6. 媒体查询。为不同设备基于它们的能力定义不同的样式。
