# Bigo 一面
+ 时间: 2019年9月6日
+ 时长： 25分钟

### 自我介绍


### 1. 输出运行结果
```javascript
var name = 'weihui';
(function() {
    if (typeof name === 'undefined') {
        var name = 'bigo';
        console.log('good' + name);
    } else {
        console.log('Hello ' + name);
    }
})()
```

### 2. 输出结果
```javascript
var arr = [];
var obj = {a: 1};
arr.push(obj);
console.log(arr);
obj.a = 2;
console.log(arr);
var obj1= obj;
obj = {a: 3};
console.log(arr);
obj1.a = 4;
console.log(arr);
```

### 3. 输出运行结果
```javascript
new Promise((resolve, reject) => {
    console.log("Promise1");
    resolve();
}).then(() => {
    console.log("then1-1");
    new Promise((resolve, reject) => {
        console.log("Promise2");
        resolve();
    }).then(() => {
        console.log("then2-1");
    }).then(() => {
        console.log("then2-2");
    })
}).then(() => {
    console.log("then1-2");
})
```

### 4. 有两个数字a,b,在不使用其他变量的情况下，交换这两个变量的值

### 5. css问题：两列布局，左边固定宽度，右边适应剩余的宽度，3种方法。