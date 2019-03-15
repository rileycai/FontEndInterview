###远程桌面面试代码题

###  bilibili 面试 
不考虑兼容性且不能更改dom结构，需求如下：

  1.完成经典的上 header ，下 footer ，左边是侧边栏，右边是内容。
  
  2.去掉列表前面的 · ，列表项水平排列，注意里面的br标签需要换行，同时每两个li后有一条竖线。
  
  3.点击列表项不跳转，弹出href内的内容

[来源](https://juejin.im/post/5c878397f265da2dde07293b)  [题目](https://github.com/zhenzhencai/FontEndInterview/blob/master/Questions.html)  [答案](https://github.com/zhenzhencai/FontEndInterview/blob/master/Answer.html)

### 用setTimeout实现setInterval

```javascript
function mySetInterval(fn, millisec,count){
  function interval(){
    if(typeof count===‘undefined’||count-->0){
      setTimeout(interval, millisec);
      try{
        fn()
      }catch(e){
        t = 0;
        throw e.toString();
      }
    }
  }
  setTimeout(interval, millisec)
}
```

参考：

[用setTimeout实现setInterval](https://www.jianshu.com/p/32479bdfd851)
