## 快手一二面
时间2019年4月13日
地点:快手总部

### 自我介绍

### 讲讲项目，遇见比较有趣的点，遇到的困难

### 有向图找出最短路径，深度优先遍历，广度优先遍历

### OSI7层模型，TCP/IP模型

### 解释一下同步，异步，阻塞，非阻塞

### get和post的区别，http是基于tcp还是udp，有哪些状态码

### 说一下http的报文格式

### 斐波那契数列编程题实现，复杂度多少，有哪些缺点，如何改进。

### vue的双向绑定原理，对象是如何绑定和监听的，如果是一个数组呢?
比如 this.arr[index]="str",要触发修改属性事件。（伪数组）

### 给一段代码，大约就是10000个li节点，使用for循环给每个节点绑定时间。问是否有问题。
涉及到事件代理和块级作用域

## 二面情况

### 编程实现url缓存。功能，首次请求时，成功执行成功回调，失败执行失败回调，再次请求时，直接拿缓存。考虑同时请求的情况
request(url,success,fail);

requestcache(url,success,fail);

### 如何拿到淘宝页面的所有不重复标签
```javascript
new Set([...document.querySelectorAll('*')].map(function(item){return item.nodeName;}))
```
