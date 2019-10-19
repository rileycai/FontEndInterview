# 图森未来面试
+ 时间： 2019年8月9日 周五

## 一面-代码面试

##### 简单介绍下实习情况

##### Game One

给定一个字符串，找出不含有重复字符的 最长子串 的长度。

示例：

给定 “abcabcbb” ，没有重复字符的最长子串是 “abc” ，那么长度就是3。

给定 “bbbbb” ，最长的子串就是 “b” ，长度是1。

给定 “pwwkew” ，最长子串是 “wke” ，长度是3。请注意答案必须是一个子串，“pwke” 是 子序列 而不是子串。

##### Game Two

了解Promise.all吗？写一个实现吧
```javascript
// 现场版本
Promise.all = (arr) => {
    if(!Array.isArray(arr)){
        throw typeError('error!');
    }
    return new Promise((resolve,reject) => {
        let resValue = [];
        const len = arr.length;
        let count = 0;
        for (let [i,item] in arr){
            Promise.resolve(item).then(res=>{
                count++;
                resValue[i] = res;
                if (count === len) {
                    resolve(resValue);
                }
            },err=>{
                reject(err);
            })
        }
    })
}
```

##### Game Three

给定一个字符串生成对应的DOM树。
PS：除了字母外，只会出现(. # > +)符号
example:
html

 input                 output
 
'div.a' =>        <div class="a"></div>
'div#a' =>        <div id="a"></div>
'div.a>span.b' =>  <div class="a"><span class="b"></span></div>
'div#a>p.b+p.c' => <div id="a">
                    <p class="b"></p>
                    <p class="c"></p>
                  </div>


## 二面 - 应用实践
+ 时间： 2019年9月10日

##### 讲讲浏览器事件循环机制，宏任务有哪些？微任务有哪些？
```javascript
console.log('1')；
setTimeout(function() {
  console.log('2')
}, 0)
new Promise(resolve => {
  console.log('3')
  resolve()
  console.log('4')
})
  .then(function() {
    console.log('5')
  })
  .then(function() {
    console.log('6')
  })
console.log('7');
// 1,3,4,7,5,6,2
``` 

```javascript
setTimeout(function() {
  console.log('1');
  new Promise(resolve => {
  console.log('2')
  resolve()
  })
  .then(function() {
    console.log('3')
  })
}, 0)
setTimeout(function() {
  console.log('4');
  new Promise(resolve => {
  console.log('5')
  resolve()
  })
  .then(function() {
    console.log('6')
  })
}, 0)
// 1,2,3,4,5,6
``` 

##### 讲讲浏览器性能优化？

##### 讲讲new原理？

##### js原始数据类型有哪些？

##### 输入一个url经历了什么？

##### 讲讲BFC？

##### 了解层叠上下文吗？

##### 了解闭包吗？闭包有哪些用途？

##### 了解函数式编程吗？了解纯函数吗？

##### redux原理？讲讲redux的工作流程？

##### 如何实现三行文本，超出的省略号？给定一个字符串，如何用js实现三行文本换行呢？

##### react最新的版本做了什么改进？ 为啥删除这三个生命周期？
删除了三个生命周期

##### setState到底是异步还是同步?

##### 模拟实现一个弹窗组件？如何让层级在最顶层？

##### 你是如何理解fiber的?

##### react 和 vuex 有什么区别？vuex原理？

##### 为什么使用typescript？

##### http2 中的多路复用和http 1.1的keep-alive有什么区别？


## 三面

##### 数学：如何判断一条直线与一个三角形相交？简化： 如何判断一条直线与一个线段相交？

##### 一个xy坐标系中的无数个点，两两连线，求斜率最大的点？

##### 数据结构：k个平均长度为n的且已经排好序的数组排序？

##### 代码题： 写个正则实现一下功能？
```javascript
abs => Abs
abs.dcs.exg => Abs Dcs.exg
abs.dxv => Abs Dxv
```

##### 数据库： 讲讲数据库隔离性？

##### 详细讲讲webSocket和http有什么不同，全双工通信是什么意思？

##### TCP有哪些拥塞处理方法？

##### 详细讲进程和线程的区别？进程之间的通信方式有哪些？

##### 数据结构：了解的排序算法有哪些？详细讲讲快排？

##### 从浏览器输入一个url发生了什么？

##### 详细讲讲DNS协议？

##### 详细讲讲CORS？

##### 详细讲讲XSS和XSRF？

##### 讲讲babel编译原理？代码优化在babel编译中的哪一步？

##### 一个小程序的代码很庞大，又不想砍掉功能，如何对其进行压缩？



## 四五面
+ 时间： 10月18日周五
+ 时长： 2个小时
+ 四五面应该是前端开发人员（6人），主要考察项目工程能力和实践能力

## 六面-总监面
+ 时间： 10月18日周五
+ 时长： 1个小时
+ 总监面主要介绍业务，考察合作能力和个人的职业规划

## hr面
+ 时间： 10月18日周五
+ 时长： 20分钟
+ 问了一下实习时间，说结果会慢点，因为还有很多候选人。


