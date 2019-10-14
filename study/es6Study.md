# es6学习笔记

## 1. let 和 const 命令
+ for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。
```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}   // 输出三次abc
```
### 1.1 不存在变量提升
+ var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined。let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

### 1.2 暂时性死区
+ 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
+ ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
+ 在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”
```javascript
function bar(x = y, y = 2) {
  return [x, y];
}
bar(); // 报错
// 调用bar函数之所以报错（某些实现可能不报错），是因为参数x默认值等于另一个参数y，而此时y还没有声明，属于“死区”。
```
### 1.3 不允许重复声明
+ let不允许在相同作用域内，重复声明同一个变量。

### 1.4 为什么需要块级作用域
+ ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。
1. 内层变量可能会覆盖外层变量。
2. 用来计数的循环变量泄露为全局变量。
+ 块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

### 1.5 块级作用域与函数声明
+ ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。
+ 允许在块级作用域内声明函数。
+ 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
+ 同时，函数声明还会提升到所在的块级作用域的头部。

### 1.6 const命令
+ const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。
+ 如果真的想将对象冻结，应该使用 **Object.freeze** 方法。
+ 除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。
```javascript
var constantize = (obj) => {
    Object.freeze(obj);
    Object.key(obj).forEach( (key, i) => {
        if (typeof obj[key] === 'object') {
            constantize(obj[key]);
        }
    })
}
```

### 1.7 顶层对象的属性
+ var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。


## 2. 变量的解构赋值
### 2.1 数组解构赋值和变量解构赋值的区别
+ 数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

### 2.2 解构赋值的用途
1. 交换变量的值
```javascript
let x = 1;
let y = 2;
[x, y] = [y, x];
```

2. 从函数返回多个值。
```javascript
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();
```

3. 函数参数的定义
```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);
// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

4. 提取JSON数据

5. 函数参数的默认值
+ 指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。

6. 遍历Map结构

7. 输入模块的指定方法
```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```



```javascript

```

## 3. 函数的扩展
### 3.1 函数的默认值
+ ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
+ 参数变量是默认声明的，所以不能用let或const再次声明。
+ 指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，length属性将失真。

### 3.2 箭头函数
1. 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
4. 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

+ this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。
+ 箭头函数转成 ES5 的代码如下。


## 4. Proxy
Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
```javascript
var proxy = new Proxy(target, handler);
```


## 4. Promise 对象
+ Promise对象有以下两个特点。
1. 对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### 4.1 Promise.prototype.then()
+ Promise 实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为 Promise 实例添加状态改变时的回调函数。
```javascript
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
```

### 4.2 Promise.prototype.catch()
+ Promise.prototype.catch方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。

### 4.3 Promise.prototype.finally()
+ finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
+ finally方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是fulfilled还是rejected。这表明，finally方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。
```javascript
// 服务器使用 Promise 处理请求，然后使用finally方法关掉服务器。
server.listen(port)
  .then(function () {
    // ...
  })
  .finally(server.stop);
```

### 4.4 Promise.all()
+ Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
+ p的状态由p1、p2、p3决定:
1. 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
2. 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
```javascript
const p = Promise.all([p1, p2, p3]);
// 生成一个Promise对象的数组
const promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON('/post/' + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```

### 4.5 Promise.race()
+ Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
+ 只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

### 4.6 Promise.resolve()
+ 有时需要将现有对象转为 Promise 对象，Promise.resolve方法就起到这个作用。
1. 参数是一个 Promise 实例。 如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
2. 参数是一个thenable对象。 Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。
3. 参数不是具有then方法的对象，或根本就不是对象。 如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。
4. 不带有任何参数。 Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。

### 4.7 Promise.reject()
+ Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。

### 4.8 应用
+ 加载图片
```javascript
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    const image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```
### 4.9 Promise.try()
+ 实际开发中，经常遇到一种情况：不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管f是否包含异步操作，都用then方法指定下一步流程，用catch方法处理f抛出的错误。一般就会采用下面的写法.
```javascript
Promise.resolve().then(f)
```
+ 上面的写法有一个缺点，就是如果f是同步函数，那么它会在本轮事件循环的末尾执行。


## 5 字符串的拓展
### 5.1 模板字符串
+ 模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
+ 如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。
+ 模板字符串中嵌入变量，需要将变量名写在${}之中
+ 模板字符串之中还能调用函数。
```javascript
// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```





