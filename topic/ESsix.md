## ES6面试

参考： [ES6高频面试题目整理](https://www.cnblogs.com/fengxiongZz/p/8191503.html)

[ECMAScript 6 入门-阮一峰](http://es6.ruanyifeng.com/#docs/let)

### 1.箭头函数和普通函数的区别是什么？
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

### 2.讲一下let、var、const的区别，谈谈如何冻结变量
+ **var** 没有块级作用域，支持变量提升。
+ **let** 有块级作用域，不支持变量提升。不允许重复声明，暂存性死区。不能通过window.变量名进行访问.
+ **const** 有块级作用域，不支持变量提升，不允许重复声明，暂存性死区。声明一个变量一旦声明就不能改变，改变报错。const保证的变量的 **内存地址** 不得改动。如果想要将对象冻结的话，使用Object.freeze()方法.
```JavaScript
//递归冻结所有的对象
var constantize=(obj)=>{
  Object.freeze(obj);
  Object.key(obj).forEach((key,i)=>
    if(typeof obj[key]==='object'){
      constantize(obj[key]);
    }  
  )}
```
+ let、const、class命令声明的全局变量，不属于顶层对象的属性。
```javascript
var a = 1;
window.a // 1
let b = 1;
window.b // undefined
```

### 3. 讲讲set结构
+ es6方法,Set本身是一个 **构造函数**，它类似于数组，但是成员值都是唯一的。
```JavaScript
const set = new Set([1,2,3,4,4])
console.log([...set] )// [1,2,3,4]
console.log(Array.from(new Set([2,3,3,5,6]))); //[2,3,5,6]
```

### 4.去除重复数组，去除字符串里面的重复字符
```JavaScript
// 去除数组的重复成员
[...new Set(array)]
//去除字符串里面的重复字符
[...new Set('ababbc')].join('')
```

### 5.如何拦截变量属性访问
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

### 6.忽略enumerable为false的属性的四个操作？
对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。描述对象的enumerable属性，称为“可枚举性”，如果该属性为false，就表示某些操作会忽略当前属性。
+ for...in循环：只遍历对象自身的和继承的可枚举的属性。
+ Object.keys()：返回对象自身的所有可枚举的属性的键名。
+ JSON.stringify()：只串行化对象自身的可枚举的属性。
+ Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

### 7.属性的五种遍历方法？
+ for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。
+ Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。
+ Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
+ Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。
+ Reflect.ownKeys返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

### 8.CommonJS模块与es6模块的差异？
+ CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
+ CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
+ CommonJS 是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。而es6模块是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响

### 9.map、filter和reduce的区别
+ map 作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变换然后放入到新的数组中。map 的回调函数接受三个参数，分别是当前索引元素，索引，原数组
+ filter 的作用也是生成一个新数组，在遍历数组的时候将返回值为 true 的元素放入新数组，我们可以利用这个函数删除一些不需要的元素
+ reduce 可以将数组中的元素通过回调函数最终转换为一个值。对于 reduce 来说，它接受两个参数，分别是回调函数和初始值。回调函数接受四个参数，分别为累计值、当前元素、当前索引、原数组
```javascript
const sum = arr.reduce((acc, current) => acc + current, 0)
```
### 10.async 及 await 的特点，它们的优点和缺点分别是什么？await 原理是什么？
+ 一个函数如果加上 async ，那么该函数就会返回一个 Promise。async 就是将函数返回值使用 Promise.resolve() 包裹了下，和 then 中处理返回值一样，并且 await 只能配套 async 使用。
+ async 和 await 可以说是异步终极解决方案了，相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码，毕竟写一大堆 then 也很恶心，并且也能优雅地解决回调地狱问题。当然也存在一些缺点，因为 await 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低。

### 11.setTimeout、setInterval、requestAnimationFrame 各有什么特点？
+ setTimeout(code, millseconds) 用于延时执行参数指定的代码，如果在指定的延迟时间之前，你想取消这个执行，那么直接用clearTimeout(timeoutId)来清除任务，timeoutID 是 setTimeout 时返回的；
+ setInterval(code, millseconds)用于每隔一段时间执行指定的代码，永无停歇，除非你反悔了，想清除它，可以使用 clearInterval(intervalId)，这样从调用 clearInterval 开始，就不会在有重复执行的任务，intervalId 是 setInterval 时返回的；
+ requestAnimationFrame(code)，一般用于动画，与 setTimeout 方法类似，区别是 setTimeout 是用户指定的，而 requestAnimationFrame 是浏览器刷新频率决定的，一般遵循 W3C 标准，它在浏览器每次刷新页面之前执行。
+ setTimeout和setInterval的问题是，它们都不精确。它们的内在运行机制决定了时间间隔参数实际上只是指定了把动画代码添加到浏览器UI线程队列中以等待执行的时间。如果队列前面已经加入了其他任务，那动画代码就要等前面的任务完成后再执行。requestAnimationFrame采用系统时间间隔，保持最佳绘制效率，不会因为间隔时间过短，造成过度绘制，增加开销；也不会因为间隔时间太长，使用动画卡顿不流畅，让各种网页动画效果能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果。著作权归作者所有。

### 12. async函数对 Generator 函数的改进
1. 内置执行器
Generator 函数的执行必须靠执行器，所以才有了co模块，而async函数自带执行器。
2. 更好的语义
async和await，比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
3. 更广的适用性
co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。
4. 返回值是Promise
async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用then方法指定下一步的操作。


### 13. 如何让await同时触发
```javascript
// getFoo完成以后，才会执行getBar
let foo = await getFoo();
let bar = await getBar();
// 方法一： promise.all
let [foo, bar] = await Promise.all([getFoo(), getBar()]);
// 方法二： 
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

### 14. 介绍下 Promise.all 使用、原理实现及错误处理
+ Promise.all()接受一个由promise任务组成的数组，可以同时处理多个promise任务，当所有的任务都执行完成时，Promise.all()返回resolve，但当有一个失败(reject)，则返回失败的信息，即使其他promise执行成功，也会返回失败。
```javascript
Promise.all = function (list) {
    if(!Array.isArray(list)){
        return reject(new TypeError("argument must be anarray"))
    }
    return new Promise((resolve, reject) => {
        let resValues = [];
        let counts = 0;
        let len = list.length;
        for (let i=0; i<len; i++) {
            Promise.resolve(list[i]).then(res => {
                counts++;
                resValues[i] = res;
                if (counts === len) {
                    resolve(resValues)
                }
            }, err => {
                reject(err)
            })
        }
    })
}
```

### 15. 设计并实现promise.race
```javascript
Promise._race = promises => new Promise((resolve, reject) => {
	promises.forEach(promise => {
		promise.then(resolve, reject)
	})
})
```
### 16. 设计并实现promise.finally
+ finally() 方法返回一个Promise。在promise结束时，无论结果是fulfilled或者是rejected，都会执行指定的回调函数。这为在Promise是否成功完成后都需要执行的代码提供了一种方式。
```javascript
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```

### 17. WeakSet 与 Set 的区别
+ WeakSet 只能储存对象引用，不能存放值，而 Set 对象都可以
+ WeakSet 对象中储存的对象值都是被弱引用的，即垃圾回收机制不考虑 WeakSet 对该对象的应用，如果没有其他的变量或属性引用这个对象值，则这个对象将会被垃圾回收掉（不考虑该对象还存在于 WeakSet 中），所以，WeakSet 对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了（被垃圾回收了），WeakSet 对象是无法被遍历的（ES6 规定 WeakSet 不可遍历），也没有办法拿到它包含的所有元素