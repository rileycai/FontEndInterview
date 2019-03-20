### 蚂蚁金服一面

面试时间：2019年3月19日下午2点

时长：52分钟。

面试官人超好，给提了很多建议，怪我自己太菜了，准备不够充分。

#### 自我介绍

### 基础问题
#### CSS
##### 1. 讲一下float,清除浮动
##### 2. 移动端适配问题，讲讲rem
+ px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。
+ em是相对长度单位。相对于当前对象内文本的字体尺寸。
+ 使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是**HTML根元素**。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。
##### 3. 讲讲盒模型
+ 盒模型分为标准模型和IE模型两种
+ 盒模型的宽高只是内容（content）的宽高，而在IE模型中盒模型的宽高是内容(content)+填充(padding)+边框(border)的总宽高。
```CSS
/* 标准模型 */
box-sizing:content-box;
 /*IE模型*/
box-sizing:border-box;
```

##### 4. 边距重叠解决方案BFC (块级格式化上下文)
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

##### 5. CSS垂直居中

##### 6.宽高比4:3自适应

##### 7.让一个图片无限旋转

#### Javascript
##### 1. 讲一讲this指针
##### 2. 讲一讲原型链
##### 3. setTimeout的this，setTimeout()时间设置为0有什么作用
+ 超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向 **window对象**，在严格模式下是 **undefined**。
+ 因此，用处就在于我们可以**改变任务的执行顺序**,因为浏览器会在执行完当前任务队列中的任务，再执行setTimeout队列中积累的的任务。

##### 4.讲一讲js里面的异步操作有哪些？
1. 回调函数。 ajax典型的异步操作，利用XMLHttpRequest，回调函数获取服务器的数据传给前端。
2. 事件监听。 当监听事件发生时，先执行回调函数，再对监听事件进行改写
3. 观察者模式，也叫订阅发布模式。 多个观察者可以订阅同一个主题，主题对象改变时，主题对象就会通知这个观察者。
4. promise.
5. es7语法糖async/await
6. co库的generator函数

参考： [JS进阶 | 分析JS中的异步操作](https://www.cnblogs.com/dirkhe/p/7384743.html)

##### 5.讲一下let、var、const的区别，谈谈如何冻结变量
+ **var** 没有块级作用域，支持变量提升。
+ **let** 有块级作用域，不支持变量提升。不允许重复声明，暂存性死区。
+ **const** 有块级作用域，不支持变量提升，不允许重复声明，暂存性死区。声明一个变量一旦声明就不能改变，改变报错。const保证的变量的**内存地址**不得改动。如果想要将对象冻结的话，使用Object.freeze()方法
```JavaScript
const foo=Object.freeze({});
foo.prop=123;
console.log(foo.prop);//混杂模式undefined,不起作用
```

##### 6.数组去重

##### 7.正则表达式（）

##### 8.函数节流和函数去抖有什么区别，怎么实现

##### 9.js事件循环机制
+ 程序开始执行之后，主程序则开始执行**同步任务**，碰到**异步任务**就把它放到任务队列中,等到同步任务全部执行完毕之后，js引擎便去查看任务队列有没有可以执行的异步任务，将异步任务转为同步任务，开始执行，执行完同步任务之后继续查看任务队列，这个过程是一直 **循环**的，因此这个过程就是所谓的**事件循环**，其中任务队列也被称为事件队列。通过一个任务队列，单线程的js实现了异步任务的执行，给人感觉js好像是多线程的。


##### 10.事件捕获和事件冒泡
+ IE的事件流叫做**事件冒泡**。即事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点window对象。
+ **事件捕获**的思想是不太具体的节点应该更早的接收到事件，而在最具体的节点应该最后接收到事件。
+ “DOM2级事件”规定事件流包括三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的事件捕获，为截获事件提供了机会。然后是实际的目标接收了事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
+ 事件流是处于**目标阶段**，事件处理程序被调用的顺序是注册的顺序
+ 事件有两个属性**e.target**和**e.currentTarget**，target是真正发生事件的DOM元素，而currentTarget是当前事件发生在哪个DOM元素上。目标阶段也就是 target == currentTarget的时候。
+ 在支持addEventListener()的浏览器中，可以调用事件对象的 **stopPropagation()** 方法以阻止事件的继续传播。如果在同一对象上定义了其他处理程序，剩下的处理程序将依旧被调用，但调用 **stopPropagation()** 之后任何其他对象上的事件处理程序将不会被调用。不仅可以阻止事件在冒泡阶段的传播，还能阻止事件在捕获阶段的传播。IE9之前的IE不支持stopPropagation()方法，而是设置事件对象cancelBubble属性为true来实现阻止事件进一步传播。
+ **e.preventDefault()** 可以阻止事件的默认行为发生，默认行为是指：点击a标签就转跳到其他页面、拖拽一个图片到浏览器会自动打开、点击表单的提交按钮会提交表单。

参考：[事件冒泡、事件捕获和事件委托](https://www.cnblogs.com/Chen-XiaoJun/p/6210987.html)


##### 10.事件代理(事件委托)及其好处，如何知道是哪个节点的事件
+ 事件委托就是利用 **事件冒泡** ，只指定一个事件处理程序，就可以管理某一类型的所有事件。
+ Event对象提供了一个属性叫target，可以返回事件的目标节点，简称事件源。
```javascript
window.onload = function(){
　　var oUl = document.getElementById("ul1");
　　oUl.onclick = function(ev){
　　　　var ev = ev || window.event;
　　　　var target = ev.target || ev.srcElement;
　　　　if(target.nodeName.toLowerCase() == 'li'){
　 　　　　　　	alert(123);
　　　　　　　  alert(target.innerHTML);
　　　　}
　　}
}
```

##### 11.排序方法，有哪些讲讲原理，js的sort（）用到哪些排序算法？
+  V8 ：数组长度小于等于 22 的用插入排序，其它的用快速排序

参考： [十大经典的排序算法](https://www.cnblogs.com/onepixel/articles/7674659.html)

##### 12.懂链表吗，懂二叉树吗，懂堆吗？
##### 13.vue如何实现双向绑定

##### 14.如何拦截变量属性
+ 使用proxy。**new Proxy()** 表示生成一个 **Proxy** 实例，**target** 参数表示所要拦截的目标对象，**handler** 参数也是一个对象，用来定制拦截行为。
```javascript
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});
let obj = Object.create(proxy);
obj.time // 35
```

##### 15.箭头函数和普通函数的区别是什么？
+ 普通函数this：
1. this总是代表它的直接调用者。
2. 在默认情况下，没找到直接调用者，this指的是window。
3. 在严格模式下，没有直接调用者的函数中的this是undefined。
4. 使用call,apply,bind绑定，this指的是绑定的对象。
+ 箭头函数this：
1. 在使用=>定义函数的时候，this的指向是 **定义时所在的对象**，而不是使用时所在的对象；
2. **不能够用作构造函数**，这就是说，不能够使用new命令，否则就会抛出一个错误；
3. 不能够使用 **arguments** 对象；
4. 不能使用 **yield** 命令；


##### 16.null和undefined的区别

##### 17.讲讲es6的promise函数

##### 18.讲讲es6的generator函数

##### 19.函数柯里化理解
+ 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
+ 用途：1.延迟计算；2.参数复用；3.动态生成函数
```JavaScript
function curry (fn, currArgs) {
    return function() {
        let args = [].slice.call(arguments);
        // 首次调用时，若未提供最后一个参数currArgs，则不用进行args的拼接
        if (currArgs !== undefined) {
            args = args.concat(currArgs);
        }
        // 递归调用
        if (args.length < fn.length) {
            return curry(fn, args);
        }
        // 递归出口
        return fn.apply(null, args);
    }
}
```

##### 20.写个js继承的例子


### 网络相关
##### 1. http 1.0 1.2 2.0的区别

##### 2.post请求常见的content-type

##### 3.强制缓存和协商缓存

##### 4.跨域有哪些方法

##### 5.写一个jsonp的实现
