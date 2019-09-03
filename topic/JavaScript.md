## JavaScript部分
### 1. 谈谈对闭包的理解，闭包的用途，闭包的缺点
+ 闭包是指有权访问另外一个函数作用域中的变量的函数
+ 闭包的用途：
1. 设计私有的方法和变量。
2. 匿名函数最大的用途是创建闭包，并且还可以构建命名空间，以减少全局变量的使用。从而使用闭包模块化代码，减少全局变量的污染。
+ 闭包的缺点：
1. 闭包会使得函数中的变量都被保存在内存中，滥用闭包可能导致内存泄漏。解决方法是在函数退出之前，将不使用的局部变量全删了。
2. 闭包会在父函数外部，改变父函数内部变量的值。

![prototype](../image/js-prototype.png)

### 2. 谈谈js的垃圾回收机制
+ JavaScript拥有自动的垃圾回收机制，当一个值，在内存中失去引用时，垃圾回收机制会根据特殊的算法找到它，并将其回收，释放内存。
+ **标记清除算法：**
1. 标记阶段，垃圾回收器会从根对象开始遍历。每一个可以从根对象访问到的对象都会被添加一个标识，于是这个对象就被标识为可到达对象。
2. 清除阶段，垃圾回收器会对堆内存从头到尾进行线性遍历，如果发现有对象没有被标识为可到达对象，那么就将此对象占用的内存回收，并且将原来标记为可到达对象的标识清除，以便进行下一次垃圾回收操作。
3. 缺点：垃圾收集后有可能会造成大量的 **内存碎片**。
+ **引用计数算法：**
1. 引用计数的含义是跟踪记录每个值被引用的次数，如果没有引用指向该对象（零引用），对象将被垃圾回收机制回收。
2. 缺点： 循环引用没法回收。

### 3. 引起JavaScript内存泄漏的操作有哪些，如何防止内存泄漏？
+ 内存泄漏（Memory Leak）是指程序中己动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。
+ 虽然JavaScript 会自动垃圾收集，但是如果我们的代码写法不当，会让变量一直处于“进入环境”的状态，无法被回收。
1. 全局变量引起的内存泄漏
2. 闭包引起的内存泄漏
3. dom清空或删除时，事件未清除导致的内存泄漏
4. 子元素存在引用引起的内存泄漏
5. 被遗忘的计时器
+ 防止内存泄漏的方法：
1. 及时清除引用。
2. 使用WeakSet和WeakMap，它们对于值的引用都是不计入垃圾回收机制的，所以名字里面才会有一个"Weak"，表示这是弱引用。
```javascript
const wm = new WeakMap();
const element = document.getElementById('example');
wm.set(element, 'some information');
wm.get(element) // "some information"
```
先新建一个 Weakmap 实例。然后，将一个 DOM 节点作为键名存入该实例，并将一些附加信息作为键值，一起存放在 WeakMap 里面。这时，WeakMap 里面对element的引用就是弱引用，不会被计入垃圾回收机制。

也就是说，DOM 节点对象的引用计数是1，而不是2。这时，一旦消除对该节点的引用，它占用的内存就会被垃圾回收机制释放。Weakmap 保存的这个键值对，也会自动消失。
参考链接： [JavaScript 内存泄漏教程](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)

### 4. ajax的用途，ajax请求的五种状态
+ AJAX是“Asynchronous JavaScript And XML”的缩写，是一种实现**无页面刷新**获取服务器数据的混合技术。
+ **XMLHttpRequest** 对象是浏览器提供的一个API，用来顺畅地向服务器发送请求并解析服务器响应，当然整个过程中，浏览器页面不会被刷新。
+ .open()方法接收三个参数：请求方式（get or post），请求URL地址和是否为异步请求的布尔值。“同步”意味着一旦请求发出，任何后续的JavaScript代码不会再执行，“异步”则是当请求发出后，后续的JavaScript代码会继续执行，当请求成功后，会调用相应的回调函数。
```javascript
// 该段代码会启动一个针对“example.php”的GET同步请求。
xhr.open("get", "example.php", false)
```
+ xhr实例的readystatechange事件会监听xhr.readyState属性的变化，有以下五种变化：

| readyState | 对应常量 | 描述 |
| ------| ------| ------|
| 0(未初始化) | xhr.UNSENT | 请求已建立, 但未初始化(此时未调用open方法) |
| 1(初始化) | xhr.OPENED | 请求已建立, 但未发送 (已调用open方法, 但未调用send方法) |
| 2(发送数据) | xhr.HEADERS_RECEIVED | 请求已发送 (send方法已调用, 已收到响应头) |
| 3(数据发送中) | xhr.LOADING | 请求处理中, 因响应内容不全, 这时通过responseBody和responseText获取可能会出现错误 |
| 4(完成) | xhr.DONE | 数据接收完毕, 此时可以通过responseBody和responseText获取完整的响应数据 |

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

参考：[再也不学AJAX了！（二）使用AJAX](https://juejin.im/post/5a20b1f1f265da432529179c#heading-7)

### 5. 对JS模块化的理解,说说es6和CommonJS的差异
1. **CommonJS**: Node.js是commonJS规范的主要实践者，用module.exports定义当前模块对外输出的接口（不推荐直接用exports），用require加载模块。commonJS用同步的方式加载模块。在服务端，模块文件都存在本地磁盘，读取非常快，所以这样做不会有问题。但是在浏览器端，限于网络原因，更合理的方案是使用异步加载。
2. **AMD和require.js**: AMD规范采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。
3. **CMD和sea.js**: CMD是另一种js模块化方案，它与AMD很类似，不同点在于：AMD 推崇依赖前置、提前执行，CMD推崇依赖就近、延迟执行。
4. **ES6 Module**: ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，旨在成为浏览器和服务器通用的模块解决方案。其模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
+ **es6和CommonJS的差异**
1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

参考：[前端模块化：CommonJS,AMD,CMD,ES6](https://juejin.im/post/5aaa37c8f265da23945f365c)

### 6. javascript做类型判断的方法有哪些？
1. **typeof**，可以判断原始数据类型：undefined、boolean、string、number、symbol，但是<kbd>typeof null</kbd>的类型判断为object。对于引用类型，会判断为function、object两种类型。
2. **instanceof** ,instanceof 检测的是原型，判断一个实例是否属于某种类型。
```javascript
function instanceOf(left,right) {
    let proto = left.__proto__;
    let prototype = right.prototype
    while(true) {
        if(proto === null) return false;
        if(proto === prototype) return true;
        proto = proto.__proto__;
    }
}
```
3. **Object.prototype.toString**
+ toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的类型。
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

### 7. call、apply、bind 的区别
+ **call、apply与bind的差别：**
1. call和apply改变了函数的this上下文后便执行该函数,而bind则是返回改变了上下文后的一个函数。
+ **call、apply的区别：**
1. call和aplly的第一个参数都是要改变上下文的对象，而call从第二个参数开始以参数列表的形式展现，apply则是把除了改变上下文对象的参数放在一个数组里面作为它的第二个参数。
```javascript
fn.call(obj, arg1, arg2, arg3...);
fn.apply(obj, [arg1, arg2, arg3...]);
```

### 8. setTimeout和setInterval的区别，包含内存方面的分析？如何用setTimeout实现setInterval？
+ setTimeout表示间隔一段时间之后执行一次调用，而setInterval则是每间隔一段时间循环调用，直至clearInterval结束。
+ 内存方面，setTimeout只需要进入一次队列，不会造成内存溢出，setInterval因为不计算代码执行时间，有可能同时执行多次代码，导致内存溢出。

```javascript
const _setInterval = (fn, misc) => {
    const interval = () => {
        setTimeout(interval, misc);
        fn();
    }
    setTimeout(interval, misc);
}
```

### 9. setTimeout(fn,0)的作用？
+ setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。
+ 它在"任务队列"的尾部添加一个事件，因此要等到主线程把同步任务和"任务队列"现有的事件都处理完，才会得到执行。
+ 用处就在于我们可以 **改变任务的执行顺序**,因为浏览器会在执行完当前任务队列中的任务，再执行setTimeout队列中积累的的任务。

### 10. 谈谈对requestAnimationFrame的理解？为什么不用setTimeout？
+ 浏览器专门为动画提供的 API，让 DOM 动画、Canvas 动画、SVG 动画、WebGL 动画等有一个统一的刷新机制。
+ **按帧对网页进行重绘。** 该方法告诉浏览器希望执行动画并请求浏览器在下一次重绘之前调用回调函数来更新动画。
+ **由系统来决定回调函数的执行时机**，在运行时浏览器会自动优化方法的调用。
+ requestAnimationFrame与setTimeout的区别：
1. **提升性能，防止掉帧**： setTimeout通过设置一个间隔时间不断改变图像，达到动画效果。该方法在一些低端机上会出现卡顿、抖动现象。这是因为setTimeout任务被放进异步队列中，只有当主线程上的任务执行完以后，才会去检查该队列的任务是否需要开始执行。所以，setTimeout的实际执行时间一般比其设定的时间晚一些。使用 requestAnimationFrame 执行动画，最大优势是能保证回调函数在屏幕每一次刷新间隔中只被执行一次，这样就不会引起丢帧，动画也就不会卡顿。
2. **函数节流**：在高频事件（resize，scroll等）中，使用requestAnimationFrame可以防止在一个刷新间隔内发生多次函数执行，这样保证了流畅性，也节省了函数执行的开销。

### 11. ES6之前JavaScript如何实现继承？
+ ES6之前的继承是通过 **原型链** 来实现的，也就是每一个构造函数都会有一个prototype属性，然后如果我们调用一个实例的方法或者属性，首先会在自身寻找，然后在构造函数的prototype上寻找，而prototype本质上就是一个实例，因此如果prototype上还没有则会往prototype上的构造函数的prototype寻找，因此实现继承可以让构造函数的prototype是父级的一个实例就是以实现继承。
```JavaScript
//寄生组合继承
function Parent (name) {
  // 属性
  this.name = name || 'Sony';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Parent.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
function Child(name){
  Parent.call(this);
  this.name = name || 'Tom';
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```

### 12. 如何阻止事件冒泡和默认事件？
+ 标准的DOM对象中可以使用事件对象的<kbd>stopPropagation()</kbd>方法来阻止事件冒泡，但在IE8以下中IE的事件对象通过设置事件对象的cancelBubble属性为true来阻止冒泡；
+ 默认事件的话通过事件对象的<kbd>preventDefault()</kbd>方法来阻止，而IE通过设置事件对象的returnValue属性为false来阻止默认事件。

### 13. 如何实现图片懒加载？
+ 当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成一张大小为1*1px图片的路径（这样就只需请求一次），只有当图片出现在浏览器的可视区域内时，才设置图片真正的路径，让图片显示出来。这就是图片懒加载

参考：[滚动加载图片（懒加载）实现原理](https://www.cnblogs.com/flyromance/p/5042187.html)

#### 使用 IntersectionObserver 
+ IntersectionObserver API为开发者提供了一种可以异步监听目标元素与其祖先或视窗(viewport)处于交叉状态的方式。祖先元素与视窗(viewport)被称为根(root)。
``` javascript
const config = {
    root: null,    // 默认指向浏览器的视口，但可以是任意DOM元素
    rootMargin: '0px',  // 计算交叉时，root边界盒的偏移量
    threshold: 0.5   // 监听对象的交叉区域与边界区域的比率
}
let observer = new IntersectionObserver(fucntion(entries){
    // ...
}, config)

new IntersectionObserver(function(entries, self))

```
+ 在entries我们得到我们的回调函数作为Array是特殊类型的：IntersectionObserverEntry 首先IntersectionObserverEntry含有三个不同的矩形的信息
+ 此外,IntersectionObserverEntry还提供了isIntersecting，这是一个方便的属性,返回观察元素是否与捕获框架相交，
+ 另外,IntersectionObserverEntry提供了利于计算的遍历属性intersctionRatio:返回intersectionRect 与 boundingClientRect 的比例值.

#### 图片懒加载实现代码
+ 以加载图片为例子,我们需要将img标签中设置一个data-src属性,它指向的是实际上我们需要加载的图像,而img的src指向一张默认的图片,如果为空的话也会向服务器发送请求。
``` html
    <img src="default.jpg" data-src="www.example.com/1.jpg">
```
``` javascript
const images = document.querySelectorAll('[data-src]')
const config = {
    rootMargin: '0px',
    threshold: 0
};
let observer = new IntersectionObserver((entries, self)=>{
    entries.forEach(entry => {
        if(entry.isIntersecting){
         // 加载图像
         preloadImage(entry.target);
         // 解除观察
           self.unobserve(entry.target)
        }
    })
}， config)

images.forEach(image => {
  observer.observe(image);
});

function preloadImage(img) {
  const src = img.dataset.src
  if (!src) { return; }
  img.src = src;
}
```
参考： [实现图片懒加载](https://juejin.im/post/583b10640ce463006ba2a71a)

### 14. 什么是函数节流和函数去抖？
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

### 15.什么是深拷贝，什么是浅拷贝？
+ 浅拷贝是指仅仅复制对象的引用，而不是复制对象本身；深拷贝则是把复制对象所引用的全部对象都复制一遍。
```JavaScript
var isObject=obj=>{return (typeof obj === 'object' || typeof obj === 'function') && obj != null;}
function deepClone(obj,hash=new Map()){
  if(!isObject(obj))
    return obj;
  if(hash.has(obj))
    return hash.get(obj);
  var target=Array.isArray(obj)?[]:{};
  hash.set(obj,target);
  reflect.ownKeys(obj).foreach(key => {
      target[key] = isObejct(obj[key])? deepClone(obj[key],hash) : obj[key]
  })
  return target;
}
```

参考：[深拷贝与浅拷贝的区别，实现深拷贝的几种方法](https://www.cnblogs.com/echolun/p/7889848.html)

### 16.如何实现对一个DOM元素的深拷贝，包括元素的绑定事件？
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

### 17.Js中forEach和map方法的区别
+ forEach返回值是undefined,不可链式调用
+ map返回一个新数组，原数组不会改变。
+ 没有办法终止或者跳出forEach循环，除非抛出异常，如果想执行一个数组是否满足什么条件，可以用Array.every()或者Array.some();

### 18. script标签如何异步加载？
+ 默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到 <kbd>script</kbd> 标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。如果脚本体积很大，下载和执行的时间就会很长，因此造成浏览器堵塞，用户会感觉到浏览器“卡死”了，没有任何响应。
```javascript
<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>
```
+ **defer与async的区别是：**
1. defer要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；async一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，defer是“渲染完再执行”，async是“下载完就执行”。
2. 另外，如果有多个defer脚本，会按照它们在页面出现的顺序加载，而多个async脚本是不能保证加载顺序的。

### 19. 讲一讲js里面的异步操作有哪些？
1. 回调函数。 ajax典型的异步操作，利用XMLHttpRequest，回调函数获取服务器的数据传给前端。
2. 事件监听。 当监听事件发生时，先执行回调函数，再对监听事件进行改写
3. 观察者模式，也叫订阅发布模式。 多个观察者可以订阅同一个主题，主题对象改变时，主题对象就会通知这个观察者。
4. promise.
5. es7语法糖async/await
6. co库的generator函数

参考： [JS进阶 | 分析JS中的异步操作](https://www.cnblogs.com/dirkhe/p/7384743.html)

### 20. 讲一下let、var、const的区别，谈谈如何冻结变量
+ **var** 没有块级作用域，支持变量提升。
+ **let** 有块级作用域，不支持变量提升。不允许重复声明，暂存性死区。
+ **const** 有块级作用域，不支持变量提升，不允许重复声明，暂存性死区。声明一个变量一旦声明就不能改变，改变报错。const保证的变量的**内存地址**不得改动。如果想要将对象冻结的话，使用Object.freeze()方法
```JavaScript
const foo=Object.freeze({});
foo.prop=123;
console.log(foo.prop);//混杂模式undefined,不起作用
```

### 21. 谈谈js事件循环机制
+ 程序开始执行之后，主程序则开始执行 **同步任务**，碰到 **异步任务** 就把它放到任务队列中,等到同步任务全部执行完毕之后，js引擎便去查看任务队列有没有可以执行的异步任务，将异步任务转为同步任务，开始执行，执行完同步任务之后继续查看任务队列，这个过程是一直 **循环** 的，因此这个过程就是所谓的 **事件循环**，其中任务队列也被称为事件队列。通过一个任务队列，单线程的js实现了异步任务的执行，给人感觉js好像是多线程的。

### 22. 事件捕获和事件冒泡
+ **DOM2级事件** 规定事件流包括三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的事件捕获，为截获事件提供了机会。然后是实际的目标接收了事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
+ **事件冒泡**，即事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点window对象。
+ **事件捕获** 的思想是不太具体的节点应该更早的接收到事件，而在最具体的节点应该最后接收到事件。
+ 在支持addEventListener()的浏览器中，可以调用事件对象的 **stopPropagation()** 方法以阻止事件的继续传播。如果在同一对象上定义了其他处理程序，剩下的处理程序将依旧被调用，但调用 **stopPropagation()** 之后任何其他对象上的事件处理程序将不会被调用。不仅可以阻止事件在冒泡阶段的传播，还能阻止事件在捕获阶段的传播。IE9之前的IE不支持stopPropagation()方法，而是设置事件对象cancelBubble属性为true来实现阻止事件进一步传播。
+ **e.preventDefault()** 可以阻止事件的默认行为发生，默认行为是指：点击a标签就转跳到其他页面、拖拽一个图片到浏览器会自动打开、点击表单的提交按钮会提交表单。

参考：[事件冒泡、事件捕获和事件委托](https://www.cnblogs.com/Chen-XiaoJun/p/6210987.html)

### 23.事件代理(事件委托)及其好处，如何知道是哪个节点的事件
+ 事件委托就是利用 **事件冒泡** ，只指定一个事件处理程序，就可以管理某一类型的所有事件。
+ 事件有两个属性 **e.target** 和 **e.currentTarget** ，target是真正发生事件的DOM元素，而currentTarget是当前事件发生在哪个DOM元素上。目标阶段也就是 target == currentTarget的时候。
```javascript
window.onload = function(){
　　var oUl = document.getElementById("ul1");
　　oUl.onclick = function(ev){
　　　　var ev = ev || window.event;
　　　　var target = ev.target || ev.srcElement;
　　　　if(target.nodeName.toLowerCase() == 'li'){
　　　　　　　  alert(target.innerHTML);
　　　　}
　　}
}
```

### 24.如何拦截变量属性
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

### 25.箭头函数和普通函数的区别是什么？
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

### 26.null和undefined的区别
+ null表示"没有对象"，即该处不应该有值。典型用法是：（1）作为函数的参数，表示该函数的参数不是对象。（2）作为对象原型链的终点。
+ undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。

### 27.函数柯里化理解
+ 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
+ 用途：1.延迟计算；2.参数复用；3.动态生成函数
```JavaScript
const curry = (fn, currArgs=[]) => {
    return function() {
        let args = Array.from(arguments);
        [].push.apply(args,currArgs);
        if (args.length < fn.length) {
            return curry.call(this,fn,...args);
        }
        return fn.apply(this,args);
    }
}
```

### 28. new的实现原理
1. 创建一个空对象，构造函数中的this指向这个空对象
2. 这个新对象被执行 **原型** 连接
3. 执行构造函数方法，属性和方法被添加到this引用的对象中
4. 如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。

```JavaScript
const _new = (fn, ...args) => {
    let obj = Object.creat(fn.prototype);
    let ret = fn.apply(obj, args);
    ret instanceof Object ? ret : obj;
}
```

### 29. JS中substr与substring的区别？
+ js中substr和substring都是截取字符串中子串，非常相近，可以有一个或两个参数。
+ substr(start [，length]) 第一个字符的索引是0，start必选 length可选
+ substring(start [, end]) 第一个字符的索引是0，start必选 end可选

### 30. 举出三种判断数组的方法？
+ **Object.prototype.toString.call()**
+ **instanceof**
+ **Array.isArray()**
```javascript
Object.prototype.toString.call(an); // "[object Array]"
[]  instanceof Array; // true
Array.isArray(arr);  // true
```

### 31. 使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果？
+ sort 函数，可以接收一个函数，返回值是比较两个数的相对顺序的值
+ 根据MDN上对Array.sort()的解释，默认的排序方法会将数组元素转换为字符串，然后比较字符串中字符的UTF-16编码顺序来进行排序。所以'102' 会排在 '15' 前面。结果是[102, 15, 22, 29, 3, 8]
