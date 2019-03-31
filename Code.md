## 代码面试题

###  1. bilibili 面试
不考虑兼容性且不能更改dom结构，需求如下：

  1.完成经典的上 header ，下 footer ，左边是侧边栏，右边是内容。

  2.去掉列表前面的 · ，列表项水平排列，注意里面的br标签需要换行，同时每两个li后有一条竖线。

  3.点击列表项不跳转，弹出href内的内容

[来源](https://juejin.im/post/5c878397f265da2dde07293b)  [题目](https://github.com/zhenzhencai/FontEndInterview/blob/master/html/Questions.html)  [答案](https://github.com/zhenzhencai/FontEndInterview/blob/master/html/Answer.html)

### 2. 用setTimeout实现setInterval

```javascript
function mySetInterval(fn, millisec){
  function interval(){
    setTimeout(interval, millisec);
    fn();
  }
  setTimeout(interval, millisec)
}
```
参考：
[用setTimeout实现setInterval](https://www.jianshu.com/p/32479bdfd851)

### 3. call模拟实现
```JavaScript
Function.prototype.call2 = function (context) {
    var context = context || window;
    context.fn = this;
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    var result = eval('context.fn(' + args +')');
    delete context.fn
    return result;
}
```
### 4. apply模拟实现
```JavaScript
Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;
    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }
    delete context.fn
    return result;
}
```
### 5. bind模拟实现
```JavaScript
Function.prototype.bind2 = function (context) {
    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);
    var fNOP = function () {};
    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }
    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

### 6. new模拟实现
```JavaScript
function create() {
	// 创建一个空的对象
    var obj = new Object(),
	// 获得构造函数，arguments中去除第一个参数
    Con = [].shift.call(arguments);
	// 链接到原型，obj 可以访问到构造函数原型中的属性
    obj.__proto__ = Con.prototype;
	// 绑定 this 实现继承，obj 可以访问到构造函数中的属性
    var ret = Con.apply(obj, arguments);
	// 优先返回构造函数返回的对象
	return ret instanceof Object ? ret : obj;
};
```

### 7. 两个升序数组合并成一个升序数组
```JavaScript
function merge(left, right){
    let result  = [],
        il      = 0,
        ir      = 0;
    while (il < left.length && ir < right.length) {
        result.push(left[il] < right[ir] ? left[il++] : right[ir++]);
    }
    return result.concat(left.slice(il)).concat(right.slice(ir));
}
```

### 8. 数组去重
```javascript
//优化遍历数组法
function distinct(arr){
    var temp=[];
    var len=arr.length;
    for(var i=0;i<len;i++){
        for(var j=i+1;j<len;j++){
            if(arr[i]===arr[j]){
                i++;
                j=i;
            }
        }
        temp.push(arr[i]);
    }
    return temp;
}
```

### 9. 给定两个数组，写一个方法来计算它们的交集。
```javascript
function intersect(arr1,arr2){
    var map=new Map();
    var arr=[];
    for(var i=0;i<arr1.length;i++){
        var temp=map.get(arr1[i]);
        map.set(arr1[i],(temp?temp:0)+1);
    }
    for(var i=0;i<arr2.length;i++){
        if(map.has(arr2[i]) &&map.get(arr2[i])!=0){
            arr.push(arr2[i]);
            map.set(arr2[i],map.get(arr2[i])-1)
        }
    }
    return arr;
}
```

### 10.promise实现ajax
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

### 11. 返回字符串中重复最多的字符
```JavaScript
function count(arr){
    var map =new Map();
    for(var i=0;i<arr.length;i++){
        if(map.get(arr[i])){
            map.set(arr[i],map.get(arr[i])+1)
        }else{
            map.set(arr[i],1);
        }
    }
    var max=0;
    var val='';
    map.forEach(function(value,key,map){
        if(value>max){
            max=value;
            val=key;
        }
    })
    return val;
}
```
### 12. 生成n为随机字符串
```JavaScript
function random(length){
    var arr=Math.random().toString(36).substr(2);
    if(arr.length>length)
        return arr.substr(0,length);
    return arr+=random(length-arr.length);
}
```

### 13. 给定一个没有重复数字的序列，返回所有的全排列
```JavaScript
var hash,res,ans,len;
function dfs(num,arr){
    if(num===len){
        var temp=res.map(function(item){
            return item;
        });
        ans.push(temp);
        return;
    }
    for(var i=0;i<len;i++){
        if(hash[i])
            continue;
        hash[i]=true;
        res.push(arr[i]);
        dfs(num+1,arr);
        hash[i]=false;
        res.pop();
    }
}
function permute(arr){
    hash=[];
    res=[];
    ans=[];
    len=arr.length;
    dfs(0,arr);
    return ans;
}
```

### 如何实现一个深拷贝？
```JavaScript
var isObject=obj=>{return typeof obj === 'object' && obj != null;}
function deepClone(obj,hash=new Map()){
  if(!isObject(obj))
    return obj;
  if(hash.has(obj))
    return hash.get(obj);
  var target=Array.isArray(obj)?[]:{};
  hash.set(obj,target);
  for(var i in obj){
    if(Object.prototype.hasOwnProperty.call(obj,i)){
      if(isObject(obj[i]))
        target[i]=deepClone(obj[i],hash);
      else {
        target[i]=obj[i];
      }
    }
  }
  return target;
}
```
### 实现双向数据绑定
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

### 手写快速排序算法
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

### 实现堆排序
```JavaScript
function swap(arr,i,j){
    let temp=arr[i];
    arr[i]=arr[j];
    arr[j]=temp;
}
function shiftDown(arr,i,len){
    let temp=arr[i];
    for(let j=2*i+1;j<len;j=2*j+1){
        temp=arr[i];
        if(j+1<len && arr[j]<arr[j+1])
            j++;
        if(arr[j]>temp){
            swap(arr,i,j);
            i=j;
        }
        else
            break;
    }
}
function heapSort(arr){
    len=arr.length;
    for(let i=Math.floor(len/2-1);i>=0;i--)
        shiftDown(arr,i,len);
    for(let i=len-1;i>=0;i--){
        swap(arr,0,i);
        shiftDown(arr,0,i);
    }
}
```
### 函数柯里化实现
```JavaScript
function curry(fn,currArgs){
    var currArgs=currArgs || [];
    return function(){
        var args=[].slice.call(arguments);
        [].push.apply(args,currArgs);

        if(args.length<fn.length)
            return curry.call(this,fn,args);
        return fn.apply(this,args);
    }
}
```
