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
    let obj = Object.create(fn.prototype);
    let ret = fn.apply(obj, args);
    return ret instanceof Object ? ret : obj;
}
```

### 7.promise实现ajax
```Javascript
//promise 实现ajax
const ajax = (method, url, data) => {
    let request = new XMLHttpRequest();
    return new Promise((resolve, reject) => {
        request.onreadystatechange = () => {
            if(request.readyState === 4) {
                if(request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        }
        request.open(method, url);
        request.send(data);
    })
}
ajax('GET', '/api/categories').then(function (text) {   // 如果AJAX成功，获得响应内容
    log.innerText = text;
}).catch(function (status) { // 如果AJAX失败，获得响应代码
    log.innerText = 'ERROR: ' + status;
});
```

### 8.如何实现一个深拷贝？
```JavaScript
var isObject = obj => {
    return (typeof obj === 'object' || typeof obj === 'function') && obj != null;
}
const deepClone = (obj, hash = new Map()) => {
    if (!isObject(obj))
        return obj;
    if (hash.has(obj))
        return hash.get(obj);
    var target = Array.isArray(obj) ? [] : {};
    hash.set(obj, target);
    reflect.ownKeys(obj).foreach(key => {
        target[key] = isObject(obj[key]) ? deepClone(obj[key], hash) : obj[key];
    })
    return target;
}

// 复杂版本
const clone = parent => {
  // 判断类型
  const isType = (obj, type) => {
    if (typeof obj !== "object") return false;
    const typeString = Object.prototype.toString.call(obj);
    let flag;
    switch (type) {
      case "Array":
        flag = typeString === "[object Array]";
        break;
      case "Date":
        flag = typeString === "[object Date]";
        break;
      case "RegExp":
        flag = typeString === "[object RegExp]";
        break;
      default:
        flag = false;
    }
    return flag;
  };

  // 处理正则
  const getRegExp = re => {
    var flags = "";
    if (re.global) flags += "g";
    if (re.ignoreCase) flags += "i";
    if (re.multiline) flags += "m";
    return flags;
  };
  // 维护两个储存循环引用的数组
  const parents = [];
  const children = [];

  const _clone = parent => {
    if (parent === null) return null;
    if (typeof parent !== "object") return parent;

    let child, proto;

    if (isType(parent, "Array")) {
      // 对数组做特殊处理
      child = [];
    } else if (isType(parent, "RegExp")) {
      // 对正则对象做特殊处理
      child = new RegExp(parent.source, getRegExp(parent));
      if (parent.lastIndex) child.lastIndex = parent.lastIndex;
    } else if (isType(parent, "Date")) {
      // 对Date对象做特殊处理
      child = new Date(parent.getTime());
    } else {
      // 处理对象原型
      proto = Object.getPrototypeOf(parent);
      // 利用Object.create切断原型链
      child = Object.create(proto);
    }

    // 处理循环引用
    const index = parents.indexOf(parent);

    if (index != -1) {
      // 如果父数组存在本对象,说明之前已经被引用过,直接返回此对象
      return children[index];
    }
    parents.push(parent);
    children.push(child);

    for (let i in parent) {
      // 递归
      child[i] = _clone(parent[i]);
    }

    return child;
  };
  return _clone(parent);
};
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
const quickSort = (arr) => {
    if (arr.length <= 1)
        return arr;
    let mid = Math.floor(arr.length/2);
    let val = arr.splice(mid,1)[0];
    let left = [], right = [];
    for(let i=0; i<arr.length; i++) {
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
            return curry.call(this,fn,...args);
        }
        return fn.apply(this,args);
    }
}
```

### 12.写个js继承的例子
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
const debounce = (fn, delay) => {
  let timer = null;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
};
```

### 15. 函数节流
```JavaScript
const throttle = (fn, delay = 500) => {
  let startTime = Date.now();
  return (...args) => {
    let currTime = Date.now();
    if(currTime - startTime > delay){
      fn.apply(this, args);
      startTime = currTime;
    }
  }
}
const throttle = (fn, delay = 500) => {
  let flag = true;
  return (...args) => {
    if (!flag) return;
    flag = false;
    setTimeout(() => {
      fn.apply(this, args);
      flag = true;
    }, delay);
  };
};
```

### 16. 实现一个instanceof
```javascript
const _instanceOf = (left, right) => {
    let proto = left.__proto__;
    let prototype = right.prototype;
    while(true) {
        if (proto === null)
            return false;
        if (proto === prototype)
            return true;
        proto = proto.__proto__;
    }
}
```

### 17. 模拟Object.create
+ Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。
```javascript
const create = (prototype) => {
    function F() {};
    F.prototype = prototype;

    return new F();
}
```

### 18. 实现promise.all
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

### 19. 使用迭代的方式实现 flatten 函数
```javascript
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]
// 方法一
const flat = (arr) => {
    let arrs = [];
    arr.map((item) => {
        if (Array.isArray(item)) {
            arrs.push(...flat(item));
        } else {
            arrs.push(item);
        }
    });
    return arrs;
}
// 方法二
arr.toString().split(',').map(item => {return +item})
```

### 20. 实现 (5).add(3).minus(2) 功能
```javascript
Number.prototype.add = function(n) {
  return this.valueOf() + n;
};
Number.prototype.minus = function(n) {
  return this.valueOf() - n;
};
```

### 21. {1:222, 2:123, 5:888}，请把数据处理为如下结构：[222, 123, null, null, 888, null, null, null, null, null, null, null]
```javascript
let obj = {1:222, 2:123, 5:888};
const result = Array.from({ length: 12 }).map((_, index) => obj[index + 1] || null);
console.log(result)
```

### 21, 从字符串S中查找是否含有子串T
```javascript
const find = (S, T) => {
    if (S.length < T.length) {
        return -1;
    }
    for(let i=0; i< S.length - T.length; i++) {
        if (S.substr(i, T.length) === T) {
            return i;
        }
    }
    return -1;
}
```

### 22. 旋转数组，将数组向右移动k位置
```javascript
const rotate = (arr, k) => {
    const len = arr.length;
    const step = k % len;
    return arr.slice(-step).concat(arr.slice(0, len-step))
}
```

### 23. 打印0-10000之间的所有对称数
```javascript
[...Array(10000).keys()].filter(item => {
    return item.toString().length >1 && item === Number(item.toString().split('').reverse().join(''));
})
```

### 24. 移动所有的零到末尾
```javascript
const moveZero = (arr) => {
    const len = arr.length;
    for(let j=0,i=0; j<len-i;j++) {
        if (arr[j] === 0) {
            arr.splice(j,1);
            arr.push(0);
            j--;
            i++;
        }
    }
    return arr;
}
```

### 25. 实现 convert 方法，把原始 list 转换成树形结构，要求尽可能降低时间复杂度
```javascript
let list =[
    {id:1,name:'部门A',parentId:0},
    {id:2,name:'部门C',parentId:1},
    {id:3,name:'部门D',parentId:1},
    {id:4,name:'部门E',parentId:3},
];
const result = convert(list, ...);

result = [{
  id:1,name:'部门A',parentId:0,
  children: [
     {id:2,name:'部门C',parentId:1},
     {id:3,name:'部门D',parentId:1,
      children:[{{id:4,name:'部门E',parentId:3},}]
     },
  ]
}]
// 答案
const convert = (list) => {
    const res = [];
    const map = list.reduce((acc, cur) => (acc[cur.id] = cur, acc),{});
    for (const item of list) {
        if (item.parentId === 0) {
            res.push(item);
            continue;
        }
        if (item.parentId in map) {
            const parent = map[item.parentId];
            parent.children = parent.children || [];
            parent.children.push(item);
        }
    }
    return res;
}
```

### 26. 输入1234 整数返回 “4321”字符串
```javascript
const reverse = (num) => {
    let num1 = num/10;
    let num2 = num%10;
    if(num<1) { 
        return num;
    } else {
        num1 = Math.floor(num1);
        return `${num2}${reverse(num1)}`
    }
}
```