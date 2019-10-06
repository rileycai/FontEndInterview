# 星环科技
+ 时间： 2019年8月28日
+ 时长： 20分钟
+ 形式： 电话


##### css如何实现一个三角形

##### 如何让溢出的文本实现省略号？
text-overflow: ellipsis；

##### 如何实现固定行数的文本，溢出的省略号？
```css
.text-overflow(@lines) when (@lines > 1) {
    display: -webkit-box;
    overflow: hidden;
    text-overflow: ellipsis;
    -webkit-line-clamp: @lines;
    -webkit-box-orient: vertical;
    white-space: normal;
}
```

##### 了解继承吗？继承有几种方法 ？

##### 箭头函数和普通函数有什么区别？

##### 原始类型有哪些？symbol的用途？

##### Object.create 和 new 有什么区别 ？

##### 如何实现new ？

##### typescript 实现了哪些便利？

##### 算法题： 找出两个数组中的重复数组？