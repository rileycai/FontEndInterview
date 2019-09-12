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
- 在表单方面，为了增强表单，为input增加color、email、date、range、url等类型，
- 在存储方面提供了sessionStorage、localStorage和离线存储，通过这些存储方式方便数据在客户端的存储和获取，
- 在多媒体方面规定了音频和视频元素audio和video；
- 另外还有地理定位、canvas画布、拖放、多线程编程的web workers和websocket协议

### 3.HTML5的存储方案有哪些
+ HTML5提供了sessionStorage、localStorage和离线存储作为新的存储方案，其中sessionStorage和localStorage都是采用键值对的形式存储，两者都是通过setItem、getItem、removeItem来实现增删查改，而sessionStorage是会话存储，也就是说当浏览器关闭之后sessionStorage也自动清空了，而localStorage不会，它没有时间上的限制。离线存储也就是应用程序缓存，这个通常用来确保web应用能够在离线情况下使用，通过在html标签中属性manifest来声明需要缓存的文件，这个属性的值是一个包含需要缓存的文件的文件名的文件，这个manifest文件声明的缓存文件可在初次加载后缓存在客户端，可以通过更新这个manifest文件来达到更新缓存文件的目的。

### 4.块级元素和行内元素有哪些？有哪些区别？
+ 块级元素有表示布局类的div、section、header、footer、aside、nav、article等，列表类ul li、ol之类的，form、p、table、td、tr，标题h1~h6
+ 行内元素：a、span、button、input、select、textarea、i、em、strong。

### 5.什么是web语义化，有什么好处？
+ web语义化：让机器可以读懂内容。有以下好处：
1. 开发者友好：使用语义类标签增强了可读性，开发者也能够清晰地看出网页的结构，也更为便于团队的开发和维护。
2. 搜索引擎友好： 有利于SEO，有利于搜索引擎 **爬虫** 更好的理解我们的网页，从而获取更多的有效信息，提升网页的权重。
3. 机器友好：语义类还可以支持读屏软件，根据文章可以自动生成目录。方便特殊群体阅读信息，比如屏幕阅读器/盲人阅读器对 **strong** 会有一个加重的读音。

### 6. meta标签的定义和作用？
+ meta标签是head部的一个辅助性标签，提供关于 HTML 文档的元数据。它并不会显示在页面上，但对于机器是可读的。可用于浏览器（如何显示内容或重新加载页面），搜索引擎（SEO），或其他 web 服务。
+ meta标签里的数据是供机器解读的，其主要作用有：搜索引擎优化（SEO），定义页面使用语言，自动刷新并指向新的页面，实现网页转换时的动态效果，控制页面缓冲，网页定级评价，控制网页显示的窗口等等。
+ **http-equiv属性**
1. **charset** 用以说明网页制作所使用的文字以及语言
2. **cache-control、Pragma、Expires** 设置网页的过期时间，一旦过期则必须到服务器上重新获取。
3. **refresh** 定时让网页在指定的时间n内，跳转到页面
4. **set-cookie** Cookie设定，如果网页过期，存盘的cookie将被删除。 
+ **name属性**
```html
<!-- 设定字符集 -->
<meta charset="utf-8">
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
 
<!-- 页面关键词 keywords -->
<meta name="keywords" content="your keywords">
 
<!-- 页面描述内容 description -->
<meta name="description" content="your description">
 
<!-- 定义网页作者 author -->
<meta name="author" content="author,email address">
 
<!-- 定义网页搜索引擎索引方式，robotterms 是一组使用英文逗号「,」分割的值，通常有如下几种取值：none，noindex，nofollow，all，index和follow。 -->
<meta name="robots" content="index,follow">
 
<!-- 优先使用最新的chrome版本 -->
<meta http-equiv="X-UA-Compatible" content="chrome=1" />
 
<!-- 禁止自动翻译 -->
<meta name="google" value="notranslate">
 
<!-- 禁止转码 -->
<meta http-equiv="Cache-Control" content="no-transform">
 
<!-- 选择使用的浏览器解析内核 -->
<meta name="renderer" content="webkit|ie-comp|ie-stand">
 
<!-- 移动端 -->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="format-detection"content="telephone=no, email=no" />
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
<meta name="apple-mobile-web-app-capable" content="yes" /><!-- 删除苹果默认的工具栏和菜单栏 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" /><!-- 设置苹果工具栏颜色 -->
<meta name="format-detection" content="telphone=no, email=no" /><!-- 忽略页面中的数字识别为电话，忽略email识别 -->
<!-- 启用360浏览器的极速模式(webkit) -->
<meta name="renderer" content="webkit">
<!-- 避免IE使用兼容模式 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">
<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
<!-- windows phone 点击无高光 -->
<meta name="msapplication-tap-highlight" content="no">

```

