## CSS部分面试题
参考资料：
[50道css基础面试题](https://segmentfault.com/a/1190000013325778)
[50道CSS基础面试题中的答案真的就只是答案吗](https://segmentfault.com/a/1190000013860482)
[css面试总结](https://funteas.com/topic/5ada8eac230d1e5e25e45b89)

### 1. 解释一下display的几个常用的属性值，inline，block，inline-block
+ inline（行内元素）:
1. 使元素变成行内元素，拥有行内元素的特性，即可以与其他行内元素共享一行，不会独占一行. 
2. 不能更改元素的height，width的值，大小由内容撑开. 
3. 可以使用padding上下左右都有效，margin只有left和right产生边距效果，但是top和bottom就不行.
+ block（块级元素）:
1. 使元素变成块级元素，独占一行，在不设置自己的宽度的情况下，块级元素会默认填满父级元素的宽度. 
2. 能够改变元素的height，width的值. 
3. 可以设置padding，margin的各个属性值，top，left，bottom，right都能够产生边距效果.
+ inline-block（融合行内于块级）:
1. 可以和其他元素共享一行；
2. 能够改变元素的height，width的值. 
3. 可以设置padding，margin的各个属性值，top，left，bottom，right都能够产生边距效果.

### 2. 介绍flex布局，用flex实现三列布局
Flex是Flexible Box的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为Flex布局。
+ 容器的属性:
1. flex-direction: row | row-reverse | column | column-reverse;
2. flex-wrap: nowrap | wrap | wrap-reverse;
3. flex-flow: flex-direction || flex-wrap;
4. justify-content: flex-start(默认) | flex-end | center | space-between | space-around;
5. align-items: flex-start | flex-end | center | baseline | stretch(默认);
6. align-content: flex-start | flex-end | center | space-between | space-around | stretch;

+ 项目的属性：
1. order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
2. flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
3. flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
4. flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
5. align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
6. flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。

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

参考：[垂直居中](https://www.cnblogs.com/hutuzhu/p/4450850.html)

### 5. 简要介绍一下CSS3的新特性
1. 在布局方面新增了flex布局；
2. 在选择器方面新增了例如:first-of-type,nth-child等选择器；
3. 在盒模型方面添加了box-sizing来改变盒模型，
4. 在动画方面增加了animation、2d变换、3d变换等。在颜色方面添加透明、rgba等，
5. 在字体方面允许嵌入字体和设置字体阴影，同时当然也有盒子的阴影，
6. 媒体查询。为不同设备基于它们的能力定义不同的样式。
```css
@media screen and (min-width:960px) and (max-width:1200px){
	body{
		background:yellow;
	}
}
```

#### 6. 讲讲盒模型
+ 盒模型分为标准模型和IE模型两种。
+ 盒模型从内到外分别是content，padding，border，margin。
+ 盒模型的宽高只是内容（content）的宽高，而在IE模型中盒模型的宽高是内容(content)+填充(padding)+边框(border)的总宽高。
```
/* 标准模型 */
box-sizing:content-box;
 /*IE模型*/
box-sizing:border-box;
```

### 7.position定位方式
+ **absolute**  生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。如果不存在就逐级向上排查，直到相对于body元素，即相对于浏览器窗口。
+ **fixed**  生成绝对定位的元素，相对于浏览器窗口进行定位。
+ **relative**  生成相对定位的元素，相对于其正常位置进行定位。
+ **static**  默认值。没有定位，元素出现在正常的流中
+ **inherit**  规定应该从父元素继承 position 属性的值。

### 8. CSS选择器分类
+ 不同级别：排序：!important > 行内样式 > ID选择器 > 类选择器=伪类=属性 > 标签=伪元素 > 通配符 > 继承 > 浏览器默认属性
+ 同一级别：后写的会覆盖先写的。
+ 联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。

### 9.使元素消失的方法
+ **opacity：0**：该元素隐藏起来，但不会改变页面布局，如果该元素绑定了事件会触发。
+ **visibility:hidden**：该元素隐藏起来，但不会改变页面布局，不会触发该元素已经绑定的事件。
+ **display:node**：把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删掉。
+ display: none和opacity: 0：是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示。 visibility: hidden：是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式。

### 10.如何画一个三角形
左右边框设置为透明，长度为底部边框的一半。左右边框长度必须设置，不设置则只有底部一条边框，是不能展示的。
```css
.child {
  	width: 0;
    height: 0;
    border-bottom: 100px solid cyan;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
}
```

### 11.SVG和Canvas的区别
+ **Canvas：** 依赖分辨率；不支持事件处理器；弱的文本渲染能力；能够以 .png 或 .jpg 格式保存结果图像；最适合图像密集型的游戏，其中的许多对象会被频繁重绘
+ **SVG：** 不依赖分辨率；支持事件处理器；最适合带有大型渲染区域的应用程序（比如谷歌地图）；复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）；不适合游戏应用

### 12.边距重叠解决方案BFC (块级格式化上下文)
+ 一个HTML元素要创建BFC，则满足下列的任意一个或多个条件即可：
1. float的值不是none。
2. position的值不是static或者relative。
3. display的值是inline-block、table-cell、flex、table-caption或者inline-flex
4. overflow的值不是visible

+ 用处：
1. 利用BFC避免外边距折叠
2. BFC包含浮动。浮动元素是会脱离文档流，如果一个没有高度或者height是auto的容器的子元素是浮动元素，则该容器的高度是不会被撑开的。我们通常会利用伪元素(:after或者:before)来解决这个问题。**BFC能包含浮动，也能解决容器高度不会被撑开的问题**。
3. 使用BFC避免文字环绕。
4. 在多列布局中使用BFC。

参考： [什么是BFC](https://www.cnblogs.com/libin-1/p/7098468.html)

### 13.宽高比4:3自适应
+ **垂直方向的padding**: 在css中，padding-top或padding-bottom的百分比值是根据容器的width来计算的。
```css
.wrap{
       position: relative;
       height: 0;  //容器的height设置为0
       width: 100%;
       padding-top: 75%;  //100%*3/4
}
.wrap > *{
       position: absolute;//容器的内容的所有元素absolute,然子元素内容都将被padding挤出容器
       left: 0;
       top: 0;
       width: 100%;
       height: 100%;
}
```
+ **padding & calc()**: 跟第一种方法原理相同
```css
padding-top: calc(100%*9/16);
```
+ **padding & 伪元素**
+ **视窗单位**： 浏览器100vw表示浏览器的视窗宽度
```javaScript
	width:100vw;
	height:calc(100vw*3/4)
```

参考： [CSS实现长宽比的几种方案](https://www.w3cplus.com/css/aspect-ratio.html)

### 14.让一个图片无限旋转
```JavaScript
 <img class="circle" src="001.jpg" width="400" height="400"/>

 //infinite 表示动画无限次播放 linear表示动画从头到尾的速度是相同的
 .circle{
         animation: myRotation 5s linear infinite;
     }
@keyframes myRotation {
         from {transform: rotate(0deg);}
         to {transform: rotate(360deg);}
}
```

### 15. 清除浮动有哪些方法
```html
<style>
    .inner {
        width: 100px;
        height: 100px;
        float: left;
    }
</style>
<div class='outer'>
    <div class='inner'></div>
    <div class='inner'></div>
    <div class='inner'></div>
</div>
```
1. 利用clear属性
+ 在 `<div class='outer'>` 内创建一个空元素，对其设置 clear: both; 的样式。
+ 优点：简单，代码少，浏览器兼容性好。
+ 缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。

2. 利用 clear 属性 + 伪元素
```javascript
.outer::after{
    content: '';
    display: block;
    clear: both;
    visibility: hidden;
    height: 0;
}
```
3. 利用BFC布局规则
+ position 为 absolute 或 fixed
+ overflow 不为 visible 的块元素
+ display 为 inline-block, table-cell, table-caption

### less和scss的区别是啥？
1. 编译环境不一样，Sass的安装需要Ruby环境，是在服务端处理的，而Less是需要引入less.js来处理Less代码输出css到浏览器，也可以在开发环节使用Less，然后编译成css文件，直接放到项目中，也有 Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。
2. 变量符不一样，Less是@，而Scss是$，而且变量的作用域也不一样。
3. 输出设置，Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。
4. Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。
5. scss引用的外部文件命名必须以_开头, 如下例所示:其中_test1.scss、_test2.scss、_test3.scss文件分别设置的h1 h2 h3。文件名如果以下划线_开头的话，Sass会认为该文件是一个引用文件，不会将其编译为css文件.

### 如何用 css 或 js 实现多行文本溢出省略效果，考虑兼容性？
+ 这样写，就算在不是webkit内核的浏览器，也可以 **优雅降级**（高度=行高*行数（webkit-line-clamp）
```css
div {
    width: 400px;
    margin: 0 auto;
    overflow: hidden;
    border:1px solid #ccc;
    text-overflow: ellipsis;  // 省略号
    padding:0 10px;
    display: -webkit-box;   
    -webkit-line-clamp: 2;    // 2行
    -webkit-box-orient: vertical;
    line-height:30px;    // 降级处理
    height:60px;
}
```
+ js实现： 通过scrollHeight与clientHeight判断是否溢出
```javascript
const p = document.querySelector('p')
let words = p.innerHTML.split(/(?<=[\u4e00-\u9fa5])|(?<=\w*?\b)/g)
while (p.scrollHeight > p.clientHeight) {
  words.pop()
  p.innerHTML = words.join('') + '...'
}
```