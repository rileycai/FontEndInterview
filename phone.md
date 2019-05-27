## 移动端面试题

### 1. click在ios上有300ms延迟，原因及如何解决？
+ **(1)粗暴型，禁用缩放**  
<meta name="viewport" content="width=device-width, user-scalable=no">
+ **(2)利用FastClick，其原理是：**
检测到touchend事件后，立刻触发模拟click事件，并且把浏览器300毫秒之后真正触发的事件给阻断掉
