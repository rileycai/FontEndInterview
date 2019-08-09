## 前端常见代码面试题

### 1. 用setTimeout实现setInterval
```javascript
const _setInterval = (fn, misc) => {
    const interval = () => {
        setTimeout(interval, misc);
        fn();
    }
    setTimeout(interval, misc);
}
```

### 2. 手写实现Promise
```javascript
// 简单版本
class Promise{
  constructor(executor){
    // 初始化state为等待态
    this.state = 'pending';
    // 成功的值
    this.value = undefined;
    // 失败的原因
    this.reason = undefined;
    let resolve = value => {
      // state改变,resolve调用就会失败
      if (this.state === 'pending') {
        // resolve调用后，state转化为成功态
        this.state = 'fulfilled';
        // 储存成功的值
        this.value = value;
      }
    };
    let reject = reason => {
      // state改变,reject调用就会失败
      if (this.state === 'pending') {
        // reject调用后，state转化为失败态
        this.state = 'rejected';
        // 储存失败的原因
        this.reason = reason;
      }
    };
    // 如果executor执行报错，直接执行reject
    try{
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
  }
  then(onFulfilled,onRejected) {
    // 状态为fulfilled，执行onFulfilled，传入成功的值
    if (this.state === 'fulfilled') {
      onFulfilled(this.value);
    };
    // 状态为rejected，执行onRejected，传入失败的原因
    if (this.state === 'rejected') {
      onRejected(this.reason);
    };
  }
}
```

参考链接： [BAT前端经典面试问题：史上最最最详细的手写Promise教程](https://juejin.im/post/5b2f02cd5188252b937548ab)

### 3. call模拟实现
```JavaScript
Function.prototype.myCall = (context = window) => {
    context.fn = this;
    let args = [...arguments].slice(1);
    let result = context.fn(...args);
    delete context.fn;
    return result;
}
```

### 4. apply模拟实现
```JavaScript
Function.prototype.myApply = (context = window) => {
    context.fn = this;
    let result = arguments[1] ? context.fn(...arguments[1]) : context.fn();
    delete context.fn;
    return result;
}
```

### 5. bind模拟实现
```JavaScript
Function.prototype.myBind = (context = window) => {
    if (typeof this != 'function') {
        throw new TypeError('error');
    }
    const _this = this;
    let args = [...arguments].slice(1);
    return function F() {
        if (this instanceof F){
            return new _this(...args, ...arguments);
        }

        return _this.apply(context, args.concat(arguments));
    }
}
```

### 6. new模拟实现
```JavaScript
const _new = (fn, ...args) => {
    let obj = Object.creat(fn.prototype);
    let ret = fn.apply(obj, args);
    ret instanceof Object ? ret : obj;
}
```

### 7.promise实现ajax
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

### 8.如何实现一个深拷贝？
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

### 9.实现双向数据绑定
```JavaScript
<body>
  <input type="text" id="message" />
  <div id="msg"></div>
</body>
<script>
   var input=document.getElementById('message');
   var show=document.getElementById('msg');
   var obj={};
   Object.defineProperty(obj,'data',{
     get:function(){
       return obj;
     },
     set:function(newValue){
        input.value=newValue;
        show.innerText=newValue;
     }
   })
   input.addEventListener('keyup',function(e){
     show.innerText=e.target.value;
   })
</script>
```

### 10.手写快速排序算法
```JavaScript
function quickSort(arr){
    if(arr.length<=1)
        return arr;
    var mid=Math.floor(arr.length/2);
    var val=arr.splice(mid,1)[0];
    var left=[],right=[];
    for(var i=0;i<arr.length;i++){
        if(arr[i]<val)
            left.push(arr[i]);
        else
            right.push(arr[i]);
    }
    return quickSort(left).concat([val]).concat(quickSort(right));
}
```

### 11.函数柯里化实现
```JavaScript
const curry = (fn, currArgs=[]) => {
    return function() {
        let args = Array.from(arguments);
        [].push.apply(args,currArgs);
        if (args.length < fn.length) {
            return curry.call(this,fn,args);
        }
        return fn.apply(this,args);
    }
}
```

### 12.写个js继承的例子
```JavaScript
//寄生组合继承
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
Cat.prototype.constructor = Cat;
```
参考： [JS继承实现方式](https://www.cnblogs.com/humin/p/4556820.html)


### 13.写一个jsonp的实现
```JavaScript
    var flightHandler = data=>{
      console.log(data);
    }
    var url = "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
    var script = document.createElement('script');
    script.setAttribute('src', url);
    document.getElementsByTagName('head')[0].appendChild(script);
```

### 14. 函数去抖
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

### 15. 函数节流
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

### 16. 实现一个instanceof
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