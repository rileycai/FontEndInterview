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

### 6.绝对定位和相对定位的区别？
绝对定位是相对于最近的已经定位的祖先元素，没有则相对于body，绝对定位脱离文档流，而相对定位是相对于元素在文档中的初始位置，并且
相对定位的元素仍然占据原有的空间。

### 7.盒子模型
+ 对于现代浏览器来说，css中指定的width就是content width。
+ 对于IE5.x和6来说，在怪异模式中width等于content、左右padding和左右border。

### 8.position定位方式
+ **absolute**  生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
+ **fixed**  生成绝对定位的元素，相对于浏览器窗口进行定位。
+ **relative**  生成相对定位的元素，相对于其正常位置进行定位。
+ **static**  默认值。没有定位，元素出现在正常的流中
+ **inherit**  规定应该从父元素继承 position 属性的值。

### 9. CSS选择器分类
+ 不同级别：排序：!important > 行内样式>ID选择器 > 类选择器=伪类=属性 > 标签 > 通配符 > 继承 > 浏览器默认属性
+ 同一级别：后写的会覆盖先写的。
+ 联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。


### 10.使元素消失的方法
+ **opacity：0**：该元素隐藏起来，但不会改变页面布局，如果该元素绑定了事件会触发。
+ **visibility:hidden**：该元素隐藏起来，但不会改变页面布局，不会触发该元素已经绑定的事件。
+ **display:node**：把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删掉。

### 11.如何画一个三角形
左右边框设置为透明，长度为底部边框的一半。左右边框长度必须设置，不设置则只有底部一条边框，是不能展示的。
```css
.child{
  	width: 0;
    height: 0;
    border-bottom: 100px solid cyan;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
}
```

### 12. CSS选择符有哪些，优先级如何计算？
+ 选择器有：id选择器、类选择器、标签选择器、相邻选择器、子选择器、后代选择器、通配符选择器、属性选择器、伪类选择器

### 13. position的值relative和absolute定位原点是？
+ **absolute**：生成绝对定位的元素，相对于值不为 static的第一个父元素进行定位。
+ **fixed （老IE不支持）** 生成绝对定位的元素，相对于浏览器窗口进行定位。
+ **relative** 生成相对定位的元素，相对于其正常位置进行定位。
+ **static** 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。
+ **inherit**  规定从父元素继承 position 属性的值。
