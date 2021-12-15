# 浏览器相关知识学习笔记

## localForage——轻松实现 Web 离线存储

- Mozilla 开发了一个叫 localForage 的库 ，使得离线数据存储在任何浏览器都是一项容易的任务。
- localForage 是一个使用非常简单的 JavaScript 库的，提供了 get，set，remove，clear 和 length 等等 API，还具有以下特点：

1. 支持回调的异步 API；
2. 支持 IndexedDB，WebSQL 和 localStorage 三种存储模式（自动为你加载最佳的驱动程序）；
3. 支持 BLOB 和任意类型的数据，让您可以存储图片，文件等等。
4. 支持 ES6 Promises；
