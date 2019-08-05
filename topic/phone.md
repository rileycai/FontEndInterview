## 移动端面试题

### 1. click在ios上有300ms延迟，原因及如何解决？
+ **(1)粗暴型，禁用缩放**  
```html
<meta name="viewport" content="width=device-width, user-scalable=no">
```
+ **(2)利用FastClick，其原理是：**
+ 检测到touchend事件后，立刻触发模拟click事件，并且把浏览器300毫秒之后真正触发的事件给阻断掉


### 2. 移动端如何适配
参考链接： [移动端如何适配](https://juejin.im/post/5cddf289f265da038f77696c)
+ 在web中，浏览器为我们提供了window.devicePixelRatio来帮助我们获取dpr。
+ 在css中，可以使用媒体查询min-device-pixel-ratio，区分dpr：
```css
@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){ }
```