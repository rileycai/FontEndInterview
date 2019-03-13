## JavaScript部分
### 1.原型与闭包

参考：
[深入理解JavaScript原型与闭包](https://www.cnblogs.com/wangfupeng1988/p/3977924.html)

[让你分分钟理解 JavaScript 闭包](https://www.cnblogs.com/onepixel/p/5062456.html)

[全面理解Javascript闭包和闭包的几种写法及用途](https://www.cnblogs.com/yunfeifei/p/4019504.html)

判断一个变量是不是对象非常简单。值类型(undefined,null, number, string, boolean)的类型判断用typeof，引用类型(函数、数组、对象)的类型判断用instanceof
```javascript
typeof null     //Object
null instanceof Object   //false
```

