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
