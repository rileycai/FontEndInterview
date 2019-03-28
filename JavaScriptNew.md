## JavaScript面试题

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
