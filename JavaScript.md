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
 
4. 每个函数function都有一个prototype,每个对象都有一个__proto__,称为隐式原型。特例：Object.prototype是对象，它的__proto__指向的是null

![](https://images0.cnblogs.com/blog/138012/201409/181510403153733.png)

5. A instancee of B,沿着A的__proto__这条线来找，同时沿着B的prototype这条线来找，如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。

```javascript
console.log(Object instanceof Function) //true
console.log(Function instanceof Object) //true
console.log(Function instanceof Function) //true
```

![](https://images0.cnblogs.com/blog/138012/201409/181637013624694.png)

6. 变量、函数表达式——变量声明，默认赋值为undefined；this——赋值；函数声明——赋值；

```javascript
console.log(f1)    //function fl(){}
function fl(){}    //函数声明
console.log(f2)    //undefined
var f2=function(){}    //函数表达式
```

7. this

1情况1：构造函数
```javascript
function Foo(){
   this.name="haha";
   this.year=1998;
   console.log(this);
}
var f1=new Foo();  // Foo {name: "haha", year: 1998}
Foo();     //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```

情况2：函数作为对象的一个属性
```javascript
var obj={
   x:10,
   fn:function(){console.log(this);console.log(this.x);}
}
obj.fn()     // {x: 10, fn: ƒ}   10
var fn1=obj.fn;    
fn1();      //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}   undefined
```

情况3：函数用call或者apply调用
```javascript
var obj={
   x:10
}
var fn=function(){console.log(this);console.log(this.x);}
fn.call(obj)      //Object {x:10}  10
```

情况4：全局 & 调用普通函数
```javascript
var obj={
   x:10,
   fn:function(){
      function f(){
         console.log(this);
         console.log(this.x);
      }
      fn();       //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}   undefined
   }
}

```

        

