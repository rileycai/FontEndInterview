# 作业帮一面
+ 时间： 2019年8月21日 下午2:30分
+ 时长： 1小时

##### 自我介绍，了解项目

##### css盒模型

##### 内联元素，块元素，内联-块元素，举例？

##### 闭包题目
```javascript
var a = 1;
function F () {
    console.log(a);
    var a = 2;
    console.log(this.a);
    this.a = 3;
}
F();  // undefined, 1
new F(); // undefined, undefined;
```

##### 原型链题目
```javascript
function A (val) {
    this.val = val || 1;
}
A.prototype.val = 2;

function B () {
    A.apply(this);
}
B.prototype.val = 3;

A.val;  // undefined
new A().val; // 1
B.val; // undefined
new B().val // 1
```

##### 代码题
```javascript
var data = [
    {name:'a',id:1,pid:0},
    {name:'b',id:2,pid:1},
    {name:'c',id:3,pid:1},
    {name:'d',id:4,pid:2},
]

// 写函数将上面data转换成如下
data = [
    {name:'a',id:1,pid:0,children:[
        {name:'b',id:2,pid:1,children:[{name:'d',id:4,pid:2,children:[]}]},
        {name:'c',id:3,pid:1,children:[]},
    ]},
]
```

##### 代码题： 字符串assdee转换成edsa，去重反转
```javascript
    let str = 'assdee';
    [...new Set(str.split(''))].reverse().join('')
```

