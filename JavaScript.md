## JavaScript部分
### 1. 原型与闭包

参考：

[深入理解JavaScript原型与闭包](https://www.cnblogs.com/wangfupeng1988/p/3977924.html)

[让你分分钟理解 JavaScript 闭包](https://www.cnblogs.com/onepixel/p/5062456.html)

[全面理解Javascript闭包和闭包的几种写法及用途](https://www.cnblogs.com/yunfeifei/p/4019504.html)

1. 判断一个变量是不是对象非常简单。值类型(undefined,null, number, string, boolean)的类型判断用typeof，引用类型(函数、数组、对象)的类型判断用instanceof，一切引用类型都是对象，对象是属性的集合。
```javascript
typeof null     //object
null instanceof Object   //false
```

2. 对象都是通过函数创建的,函数是对象
```javascript
//  var obj = { a: 10, b: 20 };
var obj = new Object();
obj.a = 10;
obj.b = 20;
console.log(typeof (Object));  // function
```

3. 每个函数都有一个属性叫做prototype。这个prototype的属性值是一个对象（属性的集合，再次强调！），默认的只有一个叫做constructor的属性，指向这个函数本身。
```javascript
function Fn() { }
   Fn.prototype.name = '王福朋';
   Fn.prototype.getYear = function () {
   return 1988;
};
var fn = new Fn();
console.log(fn.name);
console.log(fn.getYear());
```
Fn是一个函数，fn对象是从Fn函数new出来的，这样fn对象就可以调用Fn.prototype中的属性。因为每个对象都有一个隐藏的属性——“__proto__”，这个属性引用了创建这个对象的函数的prototype。即：fn.__proto__ === Fn.prototype
 
4. 每个函数function都有一个prototype,每个对象都有一个__proto__,称为隐式原型。特例：Object.prototype是对象，它的__proto__指向的是null

![](https://images0.cnblogs.com/blog/138012/201409/181510403153733.png)

5. A instancee of B,沿着A的__proto__这条线来找，同时沿着B的prototype这条线来找，如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。
```javascript
console.log(Object instanceof Function) //true
console.log(Function instanceof Object) //true
console.log(Function instanceof Function) //true
```

![](https://images0.cnblogs.com/blog/138012/201409/181637013624694.png)

6. 变量、函数表达式——变量声明，默认赋值为undefined；this——赋值；函数声明——赋值；
```javascript
console.log(f1)    //function fl(){}
function fl(){}    //函数声明
console.log(f2)    //undefined
var f2=function(){}    //函数表达式
```

7. this

1情况1：构造函数
```javascript
function Foo(){
   this.name="haha";
   this.year=1998;
   console.log(this);
}
var f1=new Foo();  // Foo {name: "haha", year: 1998}
Foo();     //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```

情况2：函数作为对象的一个属性
```javascript
var obj={
   x:10,
   fn:function(){console.log(this);console.log(this.x);}
}
obj.fn()     // {x: 10, fn: ƒ}   10
var fn1=obj.fn;    
fn1();      //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}   undefined
```

情况3：函数用call或者apply调用
```javascript
var obj={
   x:10
}
var fn=function(){console.log(this);console.log(this.x);}
fn.call(obj)      //Object {x:10}  10
```

情况4：全局 & 调用普通函数
```javascript
var obj={
   x:10,
   fn:function(){
      function f(){
         console.log(this);
         console.log(this.x);
      }
      fn();       //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}   undefined
   }
}

```

8. javascript没有块级作用域，javascript除了全局作用域之外，只有函数可以创建的作用域。作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

9. 从自由变量到作用域链。要到创建这个函数的那个作用域中取值——是“创建”，而不是“调用”
```javascript
var a=10;
function fn(){var b=20; function bar(){console.log(a+b);} return bar;}
var x=fn();
var b=200;
var x();    //30
```

10. 闭包的两种应用————函数作为返回值，函数作为参数传递。

### 2. 引起JavaScript内存泄漏的操作有哪些

虽然JavaScript 会自动垃圾收集，但是如果我们的代码写法不当，会让变量一直处于“进入环境”的状态，无法被回收。

1. 全局变量引起的内存泄漏

2. 闭包引起的内存泄漏

3. dom清空或删除时，事件未清除导致的内存泄漏

4. 子元素存在引用引起的内存泄漏

5. 被遗忘的计时器

参考：

[【译】JavaScript 内存泄漏问题](http://octman.com/blog/2016-06-28-four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/)

[JavaScript 常见的内存泄漏原因](https://juejin.im/entry/58158abaa0bb9f005873a843)

### 3. 如何实现ajax请求

(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.
```javascript
var xmlHttpRequest;  //定义一个变量,用于存放XMLHttpRequest对象
function createXMLHttpRequest()    //创建XMLHttpRequest对象的方法
{
   if(window.ActiveXObject){   //判断是否是IE浏览器
      xmlHttpRequest = new ActiveXObject("Microsoft.XMLHTTP");  //创建IE浏览器中的XMLHttpRequest对象
   }
   else if(window.XMLHttpRequest)    //判断是否是Netscape等其他支持XMLHttpRequest组件的浏览器
   {
      xmlHttpRequest = new XMLHttpRequest();  //创建其他浏览器上的XMLHttpRequest对象
   }
}
```

(2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
```javascript
XMLHttpRequest.open(method,URL,flag,name,password)
```

(3)设置响应HTTP请求状态变化的函数.

(4)发送HTTP请求.

(5)获取异步调用返回的数据.

(6)使用JavaScript和DOM实现局部刷新.

参考：

[实现AJAX的基本步骤](https://www.cnblogs.com/kennyliu/p/3876729.html)

[w3school AJAX教程](http://www.w3school.com.cn/ajax/index.asp)

### 4. Get和Post的区别

GET - 从指定的资源请求数据。 POST - 向指定的资源提交要被处理的数据。

然而，在以下情况中，请使用 POST 请求：

   无法使用缓存文件（更新服务器上的文件或数据库）

   向服务器发送大量数据（POST 没有数据量限制）
   
   发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠
 
[GET对比POST](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

### 5. 简要介绍ES6

1. 新的变量声明方式 let/const，其中最重要的两个特性就是提供了块级作用域与不再具备变量提升。同时不能重复声明。

2. 解构赋值
```javascript
let [a, [[b], c]] = [1, [[2], 3]];
let [a = 1, b] = []; // a = 1, b = undefined
let [a, ...b] = [1, 2, 3];     //a=1,b=[2,3]
//解构默认值
let [a = 3, b = a] = [];         // a = 3, b = 3
let [a = 3, b = a] = [1];        // a = 1, b = 1
let [a = 3, b = a] = [1, 2];      // a = 1, b = 2
let {a = 10, b = 5} = {a: 3};    // a = 3; b = 5;
```

3. ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。Symbol 函数栈不能用 new 命令，因为 Symbol 是原始数据类型（null,undefined,Boolean,String,Number,Symbol），不是对象。

4. ES6对字符串、 数组、正则、对象、函数等拓展了一些方法

5. 为解决异步回调问题，引入了promise和 generator
   
   Promise 异步操作有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。除了异步操作的结果，任何其他操作都无法改变这个状态。

   Promise 对象只有：从 pending 变为 fulfilled 和从 pending 变为 rejected 的状态改变。只要处于 fulfilled 和 rejected ，状态就不会再变了即 resolved（已定型）。
   
   then 方法接收两个函数作为参数，第一个参数是 Promise 执行成功时的回调，第二个参数是 Promise 执行失败时的回调，两个函数只会有一个被调用。
   
   ES6 新引入了 Generator 函数，可以通过 yield 关键字，把函数的执行流挂起，为改变执行流程提供了可能，从而为异步编程提供解决方案。

6. 实现Class和模块，通过Class可以更好的面向对象编程，使用模块加载方便模块化编程，当然考虑到浏览器兼容性，我们在实际开发中需要使用babel进行编译。

   ES6 的模块自动开启严格模式，不管你有没有在模块头部加上 use strict;。
   
   模块中可以导入和导出各种类型的变量，如函数，对象，字符串，数字，布尔值，类等。
   
   每个模块都有自己的上下文，每一个模块内声明的变量都是局部变量，不会污染全局作用域。

   每一个模块只加载一次（是单例的）， 若再去加载同目录下同文件，直接从内存中读取。
 
参考：

[ES6教程](http://www.runoob.com/w3cnote/es6-tutorial.html)

### 6. 对JS模块化的理解
   
   在ES6出现之前，js没有标准的模块化概念，这也就造成了js多人写作开发容易造成全局污染的情况，以前我们可能会采用立即执行函数、对象等方式来尽量减少变量这种情况，后面社区为了解决这个问题陆续提出了**AMD规范**和**CMD规范**，这里不同于Node.js的 CommonJS的原因在于服务端所有的模块都是存在于硬盘中的，加载和读取几乎是不需要时间的，而浏览器端因为加载速度取决于网速，因此需要采用异步加载，*AMD规范中使用define来定义一个模块，使用require方法来加载一个模块，现在ES6也推出了标准的模块加载方案，通过export和import来导出和导入模块*。
   
### 7. 介绍事件代理以及好处

   事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。好处是可以减少事件绑定，同时动态的DOM结构仍然可以监听。事件代理发生在冒泡阶段。
   
参考：

[js中的事件委托或是事件代理详解](https://www.cnblogs.com/liugang-vip/p/5616484.html)
   
   
### 8. 使用new操作符实例化一个对象的具体步骤

1.构造一个新的对象

2.将构造函数的作用域赋给新对象（也就是说this指向了新的对象）

3.执行构造函数中的代码

4.返回新对象

### 9. js如何判断网页中图片加载成功或者失败

使用onload事件运行加载成功，使用onerror事件判断失败

### 10. 递归和迭代

```javascript
// 迭代，利用变量的原值推算出变量的一个新值，迭代就是A不停的调用B.
function iteration(x){
   var sum=1; 
   for(x; x>=1; x--){
       sum = sum*x;
   }
}
// 递归：程序调用自身的编程技巧称为递归。
function recursion(x){
   if(x>1){
       return x*recursion(x-1);
   }else{
       return 1;
   }
}
```

1） 递归中一定有迭代,但是迭代中不一定有递归,大部分可以相互转换。

2） 能用迭代的不用递归,递归调用函数,浪费空间,并且递归太深容易造成堆栈的溢出./*相对*/
 
参考：

[深究递归和迭代的区别、联系、优缺点及实例对比](https://blog.csdn.net/laoyang360/article/details/7855860)

[「递归」和「迭代」有哪些区别？](https://www.zhihu.com/question/20278387)

### 11. 策略模式

意图：定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。

主要解决：在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

优点： 1、算法可以自由切换。 2、避免使用多重条件判断。 3、扩展性良好。

缺点： 1、策略类会增多。 2、所有策略类都需要对外暴露。

使用场景： 1、如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。 2、一个系统需要动态地在几种算法中选择一种。 3、如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。

参考：

[策略模式](http://www.runoob.com/design-pattern/strategy-pattern.html)

### 12. 浏览器事件循环机制

   JavaScript引擎是单线程，也就是说每次只能执行一项任务，其他任务都得按照顺序排队等待被执行，只有当前的任务执行完成之后才会往下执行下一个任务。Javascript 有一个 main thread 主线程和 call-stack 调用栈(执行栈)，所有的任务都会被放到调用栈等待主线程执行。
   
   1.JS 调用栈是一种后进先出的数据结构。当函数被调用时，会被添加到栈中的顶部，执行完成之后就从栈顶部移出该函数，直到栈内被清空。
   
   2.JavaScript 单线程中的任务分为同步任务和异步任务。同步任务会在调用栈中按照顺序排队等待主线程执行，异步任务则会在异步有了结果后将注册的回调函数添加到任务队列(消息队列)中等待主线程空闲的时候，也就是栈内被清空的时候，被读取到栈中等待主线程执行。任务队列是先进先出的数据结构。
   
   3.调用栈中的同步任务都执行完毕，栈内被清空了，就代表主线程空闲了，这个时候就会去任务队列中按照顺序读取一个任务放入到栈中执行。每次栈内被清空，都会去读取任务队列有没有任务，有就读取执行，一直循环读取-执行的操作，就形成了事件循环。
   
   4.定时器会开启一条定时器触发线程来触发计时，定时器会在等待了指定的时间后将事件放入到任务队列中等待读取到主线程执行。定时器指定的延时毫秒数其实并不准确，因为定时器只是在到了指定的时间时将事件放入到任务队列中，必须要等到同步的任务和现有的任务队列中的事件全部执行完成之后，才会去读取定时器的事件到主线程执行，中间可能会存在耗时比较久的任务，那么就不可能保证在指定的时间执行。
  
```javascript
console.log(1);
setTimeout(function() {
    console.log(2);
})
var promise = new Promise(function(resolve, reject) {
    console.log(3);
    resolve();
})
promise.then(function() {
    console.log(4);
})
console.log(5);           //1,3,5,4,2
```
   
参考：

[js浏览器事件循环机制](https://www.cnblogs.com/yqx0605xi/p/9267827.html)

### 13. 原生js操作Dom的方法有哪些？

   获取节点的方法getElementById、getElementsByClassName、getElementsByTagName、 getElementsByName、querySelector、querySelectorAll
   
   对元素属性进行操作的 getAttribute、 setAttribute、removeAttribute方法
   
   对节点进行增删改的appendChild、insertBefore、replaceChild、removeChild、 createElement等

### 14. typeof操作符返回值有哪些，对undefined、null、NaN使用这个操作符分别返回什么

   typeof的返回值有undefined、boolean、string、number、object、function、symbol。对undefined 使用返回undefined、null使用返回object，NaN使用返回number。
   
### 15. 实现一个类型判断函数，需要鉴别出基本类型、function、null、NaN、数组、对象？

只需要鉴别这些类型那么使用typeof即可，要鉴别null先判断双等判断是否为null，之后使用typeof判断，如果是obejct的话，再用Array.isArray判断
是否为数组，如果是数字再使用isNaN判断是否为NaN,（需要注意的是NaN并不是JavaScript数据类型，而是一种特殊值）如下：
```javascript
function type(ele) {
  if(ele===null) {
    return null;
  } else if(typeof ele === 'object') {
    if(Array.isArray(ele)) {
      return 'array';
    } else {
      return typeof ele;
    }
  } else if(typeof ele === 'number') {
    if(isNaN(ele)) {
      return NaN;
    } else {
      return typeof ele;
    }
  } else{
    return typeof ele;
  }
}
``` 
   
### 16. javascript做类型判断的方法有哪些？
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

当一个函数 F被定义时，JS引擎会为F添加 prototype 原型，然后再在 prototype上添加一个 constructor 属性，并让其指向 F 的引用。当执行 var f = new F() 时，F 被当成了构造函数，f 是F的实例对象，此时 F 原型上的 constructor 传递到了 f 上，因此 f.constructor == F
```javascript
' '.constructor== String
```

4. tostring

toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。
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

参考：

[判断js数据类型的四种方法](https://www.cnblogs.com/onepixel/p/5126046.html)

### 17.JavaScript严格模式下有哪些不同？
+ 不允许不使用var关键字去创建全局变量，抛出ReferenceError
+ 不允许对变量使用delete操作符，抛ReferenceError
+ 不可对对象的只读属性赋值，不可对对象的不可配置属性使用delete操作符，不可为不可拓展的对象添加属性，均抛TypeError
+ 对象属性名必须唯一
+ 函数中不可有重名参数
+ 在函数内部对修改参数不会反映到arguments中
+ 淘汰arguments.callee和arguments.caller
+ 不可在if内部声明函数
+ 抛弃with语句

参考：

1.[javascript高级程序设计](https://book.douban.com/subject/10546125/)

### 18.setTimeout和setInterval的区别，包含内存方面的分析？
setTimeout表示间隔一段时间之后执行一次调用，而setInterval则是每间隔一段时间循环调用，直至clearInterval结束。
内存方面，setTimeout只需要进入一次队列，不会造成内存溢出，setInterval因为不计算代码执行时间，有可能同时执行多次代码，
导致内存溢出。

参考：

[JS 中settimeout和setinterval函数的区别](https://my.oschina.net/u/3636678/blog/1499852)

[setTimeout() 和 setInterval() 本质区别在哪里？](https://segmentfault.com/q/1010000005989491)

