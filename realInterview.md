## JavaScript面试题

参考： [大厂高级前端面试题汇总](https://github.com/yygmind/blog/issues/5)

### 1. 介绍PM2
PM2是node进程管理工具，可以利用它来简化很多node应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。

### 2.如何解决跨域问题
跨域解决方法： 1. CORS（跨域资源共享）； 2. JSONP跨域； 3. 图像ping跨域； 4. 服务器代理； 5. document.domain 跨域； 6. window.name 跨域； 7. location.hash 跨域； 8. postMessage 跨域；

参考： [浏览器同源策略及跨域的解决方法](https://www.cnblogs.com/laixiangran/p/9064769.html)

### 3.HTTP状态码
**200 OK**  请求已成功，请求所希望的响应头或数据体将随此响应返回。
**304 Not Modified** 服务器内容没有修改。
**307 Temporary Redirect** 重定向
**401 Unauthorized** 当前请求需要用户验证。
**404 Not Found** 请求失败。
**500 Internal Server Error** 服务器出错。
