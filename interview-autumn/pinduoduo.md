# 拼多多面试

## 一面
+ 时间： 2019年9与15日
+ 时长： 70分钟

##### 在线编程： 代码题
```javascript
let arr = [
    {
        parentId: 123,
        nodeId: 23
    },
    {
        parentId: 0,
        nodeId: 123
    },
    {
        parentId: 23,
        nodeId: 999
    },
    {
        parentId: 23,
        nodeId: 789
    }
]

=>

[
    {
        parentId: 0,
        nodeId: 123,
        children: [
            {
                parentId: 123,
                nodeId: 23,
                children: [
                    {
                        parentId: 23,
                        nodeId: 999,
                        children: []
                    },
                    {
                        parentId: 23,
                        nodeId: 789,
                        children: []
                    }
                ]
            }
        ]
    }
]
```

##### d3 和 echarts 有什么区别？ canvas 和 svg有什么区别？

##### 讲讲对react context的理解？

##### webSocket是全双工通信？如何判断掉线？

##### 如何创建一个dom元素，将其添加到body元素中，重复添加十次，会生成一个节点还是十个？

##### 类数组转换成数组有哪些方法？（说出三种），可以直接用遍历吗？es6的set可以转换成数组吗？

##### es6的map的foreach方法了解不？

##### array有哪些方法可以返回一个新的数组？

##### 讲讲array的reduce的用法？

##### 说出集中垂直居中，水平居中的方法？

##### flex用过吗？ grid用过吗？可以在grid中写flex的aligin-items之类的吗？

##### 用过TypeScript吗？在什么情景下使用，遇见过什么问题？

##### 如何判断一个变量的类型？

##### 箭头函数的this指向？

##### async await 如何抛出异常？

##### webpack 配置如何做性能优化？



## 二面

##### 自我介绍，了解项目

##### 代码题： 手写实现call,apply,bind

##### http请求头部有哪些？

##### css选择器有哪些？

##### 箭头函数跟普通函数有什么不同？箭头函数有什么具体的用处，举个实际应用？

##### 了解redux吗？redux中间件有接触过吗？

##### 太多了。。忘了。 


