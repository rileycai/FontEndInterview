## JavaScript部分
### 1.原型与闭包

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
 
4. 每个函数function都有一个prototype,每个对象都有一个__proto__,称为隐式原型

![](https://images0.cnblogs.com/blog/138012/201409/181509180812624.png)



        

