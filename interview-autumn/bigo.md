# Bigo 面试记录

## 一面
+ 时间: 2019年9月6日
+ 时长： 25分钟

##### 自我介绍

##### 1. 输出运行结果
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

##### 2. 输出结果
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

##### 3. 输出运行结果
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

##### 4. 有两个数字a,b,在不使用其他变量的情况下，交换这两个变量的值

##### 5. css问题：两列布局，左边固定宽度，右边适应剩余的宽度，3种方法。


## 二面

##### 自我介绍，聊项目？

##### js函数参数是按值传递还是按引用传递？

##### 如何判断一个元素是否在可视区域内？

##### 如何实现移动端适配？

##### 如何实现懒加载，tab标签页的懒加载呢？

##### 讲讲ajax请求？

##### 遇见过移动端touchend不响应的问题吗？如何用touchmove模拟scroll事件？

##### 节流函数怎么实现？


## 三面

##### 自我介绍，聊项目？

##### 代码题： 将一个字符串A中的子串B替换为C，不能使用js的API。

##### 输入一个url，讲讲详细的流程。

##### 强缓存和协商缓存

##### 为什么浏览器限制请求数为6个，如何优化？

