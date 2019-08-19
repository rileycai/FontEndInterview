## HTML(5) 面试题
### 1.viewport的常见设置有哪些
viewport常常使用在响应式开发以及移动web开发中，viewport顾名思义就是用来设置视口，主要是规定视口的宽度、视口的初始缩放值、
视口的最小缩放值、视口的最大缩放值、是否允许用户缩放等。一个常见的viewport设置如下：
```html
<meta name="viewport"  content="initial-scale=1,maximum-scale=1,user-scalable=no,width=device-width" />
```
其中同时设置width和initial-scale的目的是为了解决iphone、ipad、ie横竖屏不分的情况，因为这两个值同时存在时会取较大值。

### 2.简要介绍HTML5的新特性
- 首先HTML5为了更好的实践Web语义化，增加了header、footer、nav、aside、article、section等语义化标签。
- 在表单方面，为了增强表单，为input增加color、email、date、range等类型，
- 在存储方面提供了sessionStorage、localStorage和离线存储，通过这些存储方式方便数据在客户端的存储和获取，
- 在多媒体方面规定了音频和视频元素audio和vedio；
- 另外还有地理定位、canvas画布、拖放、多线程编程的web workers和websocket协议

### 3.HTML5的存储方案有哪些
+ HTML5提供了sessionStorage、localStorage和离线存储作为新的存储方案，其中sessionStorage和localStorage都是采用键值对的形式存储，两者都是通过setItem、getItem、removeItem来实现增删查改，而sessionStorage是会话存储，也就是说当浏览器关闭之后sessionStorage也自动清空了，而localStorage不会，它没有时间上的限制。离线存储也就是应用程序缓存，这个通常用来确保web应用能够在离线情况下使用，通过在html标签中属性manifest来声明需要缓存的文件，这个属性的值是一个包含需要缓存的文件的文件名的文件，这个manifest文件声明的缓存文件可在初次加载后缓存在客户端，可以通过更新这个manifest文件来达到更新缓存文件的目的。

### 4.块级元素和行内元素有哪些？有哪些区别？
+ 块级元素有表示布局类的div、section、header、footer、aside、nav、article等，列表类ul li、ol之类的，form、p、table、标题h1~h6
+ 行内元素：a、span、button、input、select、textarea、i、em、strong。

### 5.什么是web语义化，有什么好处？
+ web语义化：让机器可以读懂内容。有以下好处：
+ 有利于SEO，有利于搜索引擎**爬虫**更好的理解我们的网页，从而获取更多的有效信息，提升网页的权重。
+ 语义类还可以支持读屏软件，根据文章可以自动生成目录。
+ 在没有CSS的时候能够清晰的看出网页的结构，增强可读性。
+ 便于团队开发和维护，语义化的HTML可以让开发者更容易的看明白，从而提高团队的效率和协调能力。
+ 支持多终端设备的浏览器渲染。
+ 方便特殊群体阅读信息，比如屏幕阅读器/盲人阅读器对 **strong** 会有一个加重的读音。

### 6. HTML5与HTML4的不同之处？
+ 文件类型声明（<!DOCTYPE>）仅有一型：<!DOCTYPE HTML>。
+ 新的解析顺序：不再基于SGML。
+ 新的元素：section, video, progress, nav, meter, time, aside, canvas, command, datalist, details, embed, figcaption, figure, footer, header, hgroup, keygen, mark, output, rp, rt, ruby, source, summary, wbr。
+ input元素的新类型：date, email, url等等。
+ 新的属性：ping（用于a与area）, charset（用于meta）, async（用于script）。
+ 全域属性：id, tabindex, repeat。
+ 新的全域属性：contenteditable, contextmenu, draggable, dropzone, hidden, spellcheck。
+ 移除元素：acronym, applet, basefont, big, center, dir, font, frame, frameset, isindex, noframes, strike, tt