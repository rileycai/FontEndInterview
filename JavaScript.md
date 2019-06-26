## JavaScript部分
### 1. 原型与闭包

闭包是指有权访问另外一个函数作用域中的变量的函数

参考：[深入理解JavaScript原型与闭包](https://www.cnblogs.com/wangfupeng1988/p/3977924.html)

![](https://images0.cnblogs.com/blog/138012/201409/181637013624694.png)

### 2. 引起JavaScript内存泄漏的操作有哪些

虽然JavaScript 会自动垃圾收集，但是如果我们的代码写法不当，会让变量一直处于“进入环境”的状态，无法被回收。

1. 全局变量引起的内存泄漏

2. 闭包引起的内存泄漏

3. dom清空或删除时，事件未清除导致的内存泄漏

4. 子元素存在引用引起的内存泄漏

5. 被遗忘的计时器

参考：[JavaScript深入之4类常见内存泄漏及如何避免](https://github.com/yygmind/blog/issues/16)  [【译】JavaScript 内存泄漏问题](http://octman.com/blog/2016-06-28-four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/)  [JavaScript 常见的内存泄漏原因](https://juejin.im/entry/58158abaa0bb9f005873a843)

### 3. ajax请求的五种状态
+ 0 － （未初始化）XMLHttprequset对象已经存在
+ 1 － （载入）调用open()方法，根据参数(method,url,true)完成对象状态的设置。并调用send()方法开始向服务端发送请求。值为1表示正在向服务端发送请求。
+ 2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
+ 3 － （交互）正在解析响应内容
+ 4 － （完成）响应内容解析完成，可以在客户端调用了

```Javascript
//promise 实现ajax
function ajax(method, url, data) {
    var request = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
            if (request.readyState === 4) {
                if (request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        };
        request.open(method, url);
        request.send(data);
    });
}
ajax('GET', '/api/categories').then(function (text) {   // 如果AJAX成功，获得响应内容
    log.innerText = text;
}).catch(function (status) { // 如果AJAX失败，获得响应代码
    log.innerText = 'ERROR: ' + status;
});
```

参考：[实现AJAX的基本步骤](https://www.cnblogs.com/kennyliu/p/3876729.html)  [w3school AJAX教程](http://www.w3school.com.cn/ajax/index.asp)

### 4. 简要介绍ES6

1. 新的变量声明方式 let/const，其中最重要的两个特性就是提供了块级作用域与不再具备变量提升，同时不能重复声明。
2. 解构赋值
3. ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型，不是对象。
4. ES6对字符串、 数组、正则、对象、函数等拓展了一些方法
5. 为解决异步回调问题，引入了promise和 generator，es7引入了async
6. 实现Class和模块，通过Class可以更好的面向对象编程，使用模块加载方便模块化编程，当然考虑到浏览器兼容性，我们在实际开发中需要使用babel进行编译。

参考：[ES6教程](http://www.runoob.com/w3cnote/es6-tutorial.html)

### 5. 对JS模块化的理解
在ES6出现之前，js没有标准的模块化概念，这也就造成了js多人写作开发容易造成全局污染的情况，以前我们可能会采用立即执行函数、对象等方式来尽量减少变量这种情况，后面社区为了解决这个问题陆续提出了 **AMD规范** 和 **CMD规范** ，这里不同于Node.js的 CommonJS的原因在于服务端所有的模块都是存在于硬盘中的，加载和读取几乎是不需要时间的，而浏览器端因为加载速度取决于网速，因此需要采用异步加载，*AMD规范中使用define来定义一个模块，使用require方法来加载一个模块，现在ES6也推出了标准的模块加载方案，通过export和import来导出和导入模块*。

### 6. js如何判断网页中图片加载成功或者失败

使用onload事件运行加载成功，使用onerror事件判断失败

### 7. 策略模式

意图：定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。

主要解决：在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

优点： 1、算法可以自由切换。 2、避免使用多重条件判断。 3、扩展性良好。

缺点： 1、策略类会增多。 2、所有策略类都需要对外暴露。

使用场景： 1、如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。 2、一个系统需要动态地在几种算法中选择一种。 3、如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。

参考：[策略模式](http://www.runoob.com/design-pattern/strategy-pattern.html)

### 8. 原生js操作Dom的方法有哪些？
+ 获取节点的方法getElementById、getElementsByClassName、getElementsByTagName、 getElementsByName、querySelector、querySelectorAll
+ 对元素属性进行操作的 getAttribute、 setAttribute、removeAttribute方法
+ 对节点进行增删改的appendChild、insertBefore、replaceChild、removeChild、 createElement等

### 9. typeof操作符返回值有哪些，对undefined、null、NaN使用这个操作符分别返回什么

typeof的返回值有undefined、boolean、string、number、object、function、symbol。对undefined 使用返回undefined、null使用返回object，NaN使用返回number。

### 10. javascript做类型判断的方法有哪些？
1. typeof
```javascript
typeof '';       // string 有效
typeof 1;        // number 有效
typeof Symbol(); // symbol 有效
typeof true;     //boolean 有效
typeof undefined; //undefined 有效
typeof null;     //object 无效
typeof [] ;      //object 无效
typeof new Function();      // function 有效
typeof new Date();          //object 无效
typeof new RegExp();        //object 无效
```

2. instanceof ,instanceof 检测的是原型
```javascript
instanceof (A,B) = {
    var L = A.__proto__;
    var R = B.prototype;
    if(L === R) {
        // A的内部属性 __proto__ 指向 B 的原型对象
        return true;
    }
    return false;
}
```

3. constructor
+ 当一个函数 F被定义时，JS引擎会为F添加 prototype 原型，然后再在 prototype上添加一个 constructor 属性，并让其指向 F 的引用。当执行 var f = new F() 时，F 被当成了构造函数，f 是F的实例对象，此时 F 原型上的 constructor 传递到了 f 上，因此 f.constructor == F
```javascript
' '.constructor== String
```

4. tostring
+ toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。
```javascript
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window 是全局对象 global 的引用
```

参考：[判断js数据类型的四种方法](https://www.cnblogs.com/onepixel/p/5126046.html)

### 11.JavaScript严格模式下有哪些不同？
+ 不允许不使用var关键字去创建全局变量，抛出ReferenceError
+ 不允许对变量使用delete操作符，抛ReferenceError
+ 不可对对象的只读属性赋值，不可对对象的不可配置属性使用delete操作符，不可为不可拓展的对象添加属性，均抛TypeError
+ 对象属性名必须唯一
+ 函数中不可有重名参数
+ 在函数内部对修改参数不会反映到arguments中
+ 淘汰arguments.callee和arguments.caller
+ 不可在if内部声明函数
+ 抛弃with语句

参考：[javascript高级程序设计](https://book.douban.com/subject/10546125/)

### 12.setTimeout和setInterval的区别，包含内存方面的分析？
setTimeout表示间隔一段时间之后执行一次调用，而setInterval则是每间隔一段时间循环调用，直至clearInterval结束。
内存方面，setTimeout只需要进入一次队列，不会造成内存溢出，setInterval因为不计算代码执行时间，有可能同时执行多次代码，导致内存溢出。

参考： [setTimeout和setInterval从入门到精通](https://www.cnblogs.com/pelli/p/6225858.html)

### 13. ES6之前JavaScript如何实现继承？
ES6之前的继承是通过 **原型** 来实现的，也就是每一个构造函数都会有一个prototype属性，然后如果我们调用一个实例的方法或者属性，首先会在自身寻找，然后在
构造函数的prototype上寻找，而prototype本质上就是一个实例，因此如果prototype上还没有则会往prototype上的构造函数的prototype寻找，因此实现继承
可以让构造函数的prototype是父级的一个实例就是以实现继承。

### 14. 如何阻止事件冒泡和默认事件？
标准的DOM对象中可以使用事件对象的stopPropagation()方法来阻止事件冒泡，但在IE8以下中IE的事件对象通过设置事件对象的cancelBubble属性为true来阻止冒泡；默认事件的话通过事件对象的preventDefault()方法来阻止，而IE通过设置事件对象的returnValue属性为false来阻止默认事件。

### 15. addEventListener有哪些参数？
有三个参数，第一个是事件的类型，第二个是事件的回调函数，第三个是一个表示事件是冒泡阶段还是捕获阶段捕获的布尔值，true表示捕获，false表示冒泡。

### 16. 如何实现懒加载？

当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成一张大小为1*1px图片的路径（这样就只需请求一次），只有当图片出现在浏览器的可视区域内时，才设置图片正真的路径，让图片显示出来。这就是图片懒加载

参考：[滚动加载图片（懒加载）实现原理](https://www.cnblogs.com/flyromance/p/5042187.html)

### 17. 什么是函数节流和函数去抖？
+ **函数去抖 debounce**： 当调用函数n秒后，才会执行该动作，若在这n秒内又调用该函数则将取消前一次并重新计算执行时间。
```JavaScript
var debounce = function(delay, cb) {
    var timer;
    return function() {
        if (timer) clearTimeout(timer);
        timer = setTimeout(function() {
            cb();
        }, delay);
    }
}
```
+ **函数节流 throttle**： 函数节流的基本思想是函数预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期
```JavaScript
var throttle = function(delay, cb) {
    var startTime = Date.now();
    return function() {
        var currTime = Date.now();
        if (currTime - startTime > delay) {
            cb();
            startTime = currTime;
        }
    }
}
```
参考：[JS函数节流](https://www.cnblogs.com/mopagunda/p/5323080.html)

### 18.请说一下实现jsonp的实现思路？
jsonp的原理是使用script标签来实现跨域，因为script标签的的src属性是不受同源策略的影响的，因此可以使用其来跨域。一个最简单的jsonp就是创建一个script标签，设置src为相应的url，在url之后添加相应的callback，格式类似于url?callback=xxx，服务端根据我们的callback来返回相应的数据，类似于res.send(req.query.callback + '('+ data + ')')，这样就实现了一个最简单的jsonp

参考：[jsonp原理详解](https://blog.csdn.net/hansexploration/article/details/80314948)

### 19.浏览器内核有哪些？分别对应哪些浏览器？
常见的浏览器内核有Trident、Gecko、WebKit、Presto，对应的浏览器为Trident对应于IE，Gecko对应于火狐浏览器，Webkit有chrome和safari，Presto有Opera。

### 20.什么是深拷贝，什么是浅拷贝？
浅拷贝是指仅仅复制对象的引用，而不是复制对象本身；深拷贝则是把复制对象所引用的全部对象都复制一遍。
```JavaScript
var isObject=obj=>{return typeof obj === 'object' && obj != null;}
function deepClone(obj,hash=new Map()){
  if(!isObject(obj))
    return obj;
  if(hash.has(obj))
    return hash.get(obj);
  var target=Array.isArray(obj)?[]:{};
  hash.set(obj,target);
  for(var i in obj){
    if(Object.prototype.hasOwnProperty.call(obj,i)){
      if(isObject(obj[i]))
        target[i]=deepClone(obj[i],hash);
      else {
        target[i]=obj[i];
      }
    }
  }
  return target;
}
```

参考：[深拷贝与浅拷贝的区别，实现深拷贝的几种方法](https://www.cnblogs.com/echolun/p/7889848.html)

### 21.原生js字符串方法有哪些？
简单分为获取类方法，获取类方法有charAt方法用来获取指定位置的字符，获取指定位置字符的unicode编码的charCodeAt方法，
与之相反的fromCharCode方法，通过传入的unicode返回字符串。查找类方法有indexof()、lastIndexOf()、search()、match()
方法。截取类的方法有substring、slice、substr三个方法，其他的还有replace、split、toLowerCase、toUpperCase方法。

### 22.原生js字符串截取方法有哪些？有什么区别？
js字符串截取方法有substring、slice、substr三个方法，substring和slice都是指定截取的首尾索引值，不同的是传递负值的时候
substring会当做0来处理，而slice传入负值的规则是-1指最后一个字符，substr方法则是第一个参数是开始截取的字符串，第二个是截取的字符数量，
和slice类似，传入负值也是从尾部算起的。

### 23.将静态资源放在其他域名的目的是什么？
这样做的主要目的是在请求这些静态资源的时候不会发送cookie，节省了流量，需要注意的是cookie是会发送给子域名的（二级域名），所以这些静态资源是不会放在子域名下的，而是单独放在一个单独的主域名下。同时还有一个原因就是浏览器对于一个域名会有请求数的限制，这种方法可以方便做CDN。

参考：[为什么淘宝、腾讯等会把静态资源放在另外一个主域名下？](https://www.zhihu.com/question/20627139)  [为什么很多网站的静态资源会使用独立的域名？](https://www.zhihu.com/question/20534662)

### 24.如何实现对一个DOM元素的深拷贝，包括元素的绑定事件？
```javascript
//使用cloneNode，但是在元素上绑定的事件不会拷贝
function clone(origin) {
    return Object.assign({},origin);
}
//实现了对原始对象的克隆，但是只能克隆原始对象自身的值，不能克隆她继承的值，如果想要保持继承链，可以采用如下方法：
function clone(origin) {
    let originProto=Object.getPrototypeOf(origin);
    return Object.assign(Object.create(originProto),origin);
}
```
参考：[如何实现对一个DOM元素的深拷贝，包括元素的绑定事件？](https://blog.csdn.net/caoxinhui521/article/details/78079377)

### 25.Js中forEach和map方法的区别
+ forEach返回值是undefined,不可链式调用
+ map返回一个新数组，原数组不会改变。
+ 没有办法终止或者跳出forEach循环，除非抛出异常，如果想执行一个数组是否满足什么条件，可以用Array.every()或者Array.some();

### 26. script标签如何异步加载？
+ 默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到<script>标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。如果脚本体积很大，下载和执行的时间就会很长，因此造成浏览器堵塞，用户会感觉到浏览器“卡死”了，没有任何响应。
```javascript
<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>
```
+ defer与async的区别是：defer要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；async一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，defer是“渲染完再执行”，async是“下载完就执行”。
+ 另外，如果有多个defer脚本，会按照它们在页面出现的顺序加载，而多个async脚本是不能保证加载顺序的。

### 27.setTimeout的this，setTimeout()时间设置为0有什么作用
+ 超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向 **window对象**，在严格模式下是 **undefined**。
+ 因此，用处就在于我们可以 **改变任务的执行顺序**,因为浏览器会在执行完当前任务队列中的任务，再执行setTimeout队列中积累的的任务。

### 28.讲一讲js里面的异步操作有哪些？
1. 回调函数。 ajax典型的异步操作，利用XMLHttpRequest，回调函数获取服务器的数据传给前端。
2. 事件监听。 当监听事件发生时，先执行回调函数，再对监听事件进行改写
3. 观察者模式，也叫订阅发布模式。 多个观察者可以订阅同一个主题，主题对象改变时，主题对象就会通知这个观察者。
4. promise.
5. es7语法糖async/await
6. co库的generator函数

参考： [JS进阶 | 分析JS中的异步操作](https://www.cnblogs.com/dirkhe/p/7384743.html)

### 29.讲一下let、var、const的区别，谈谈如何冻结变量
+ **var** 没有块级作用域，支持变量提升。
+ **let** 有块级作用域，不支持变量提升。不允许重复声明，暂存性死区。
+ **const** 有块级作用域，不支持变量提升，不允许重复声明，暂存性死区。声明一个变量一旦声明就不能改变，改变报错。const保证的变量的**内存地址**不得改动。如果想要将对象冻结的话，使用Object.freeze()方法
```JavaScript
const foo=Object.freeze({});
foo.prop=123;
console.log(foo.prop);//混杂模式undefined,不起作用
```

### 30.js事件循环机制
+ 程序开始执行之后，主程序则开始执行 **同步任务**，碰到 **异步任务** 就把它放到任务队列中,等到同步任务全部执行完毕之后，js引擎便去查看任务队列有没有可以执行的异步任务，将异步任务转为同步任务，开始执行，执行完同步任务之后继续查看任务队列，这个过程是一直 **循环** 的，因此这个过程就是所谓的 **事件循环**，其中任务队列也被称为事件队列。通过一个任务队列，单线程的js实现了异步任务的执行，给人感觉js好像是多线程的。


### 31.事件捕获和事件冒泡
+ IE的事件流叫做 **事件冒泡**。即事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点window对象。
+ **事件捕获** 的思想是不太具体的节点应该更早的接收到事件，而在最具体的节点应该最后接收到事件。
+ **DOM2级事件** 规定事件流包括三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的事件捕获，为截获事件提供了机会。然后是实际的目标接收了事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
+ 事件流是处于 **目标阶段**，事件处理程序被调用的顺序是注册的顺序
+ 事件有两个属性 **e.target** 和 **e.currentTarget** ，target是真正发生事件的DOM元素，而currentTarget是当前事件发生在哪个DOM元素上。目标阶段也就是 target == currentTarget的时候。
+ 在支持addEventListener()的浏览器中，可以调用事件对象的 **stopPropagation()** 方法以阻止事件的继续传播。如果在同一对象上定义了其他处理程序，剩下的处理程序将依旧被调用，但调用 **stopPropagation()** 之后任何其他对象上的事件处理程序将不会被调用。不仅可以阻止事件在冒泡阶段的传播，还能阻止事件在捕获阶段的传播。IE9之前的IE不支持stopPropagation()方法，而是设置事件对象cancelBubble属性为true来实现阻止事件进一步传播。
+ **e.preventDefault()** 可以阻止事件的默认行为发生，默认行为是指：点击a标签就转跳到其他页面、拖拽一个图片到浏览器会自动打开、点击表单的提交按钮会提交表单。

参考：[事件冒泡、事件捕获和事件委托](https://www.cnblogs.com/Chen-XiaoJun/p/6210987.html)

### 32.事件代理(事件委托)及其好处，如何知道是哪个节点的事件
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

### 33.如何拦截变量属性
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

### 34.箭头函数和普通函数的区别是什么？
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

### 35.null和undefined的区别
+ null表示"没有对象"，即该处不应该有值。典型用法是：（1）作为函数的参数，表示该函数的参数不是对象。（2）作为对象原型链的终点。
+ undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。

### 36.函数柯里化理解
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

### 37. new的实现原理
1. 创建一个空对象，构造函数中的this指向这个空对象
2. 这个新对象被执行 [[原型]] 连接
3. 执行构造函数方法，属性和方法被添加到this引用的对象中
4. 如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。
```javascript
function _new() {
    let target = {};
    let [constructor, ...args] = [...arguments];
    target._proto_ = constructor.prototype;
    let result = constructor.apply(target, args);
    if (result && (typeof (result) == 'object' || typeof (result) == 'function')) {
        return result;
    }
    return target;
}
```
