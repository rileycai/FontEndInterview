## 浏览器相关知识点
### 1.实现一个postMessage跨域
```javascript
// 发送消息端
window.parent.postMessage('message', 'http://test.com')
// 接收消息端
var mc = new MessageChannel()
mc.addEventListener('message', event => {
  var origin = event.origin || event.originalEvent.origin
  if (origin === 'http://test.com') {
    console.log('验证通过')
  }
})
```

### 2. cookie，localStorage，sessionStorage，indexDB的区别
| 特性 | cookie | localStorage | sessionStorage | indexDB |
| ------ | ------ | ------ | ------ | ------ |
| 数据生命周期 | 一般由服务器生成，可以设置过期时间 | 除非被清理，否则一直存在 | 页面关闭就清理 | 除非被清理，否则一直存在|
| 数据存储大小 | 4k | 5M | 5M | 无限 |
| 与服务端通信 | header中，对于请求性能影响 | 不参与 | 不参与 | 不参与 |

### 3. cookie某些属性的作用？
| 属性 | 作用 |
| ------ | ------ |
| value | 如果用于保存用户登录态，应该将该值加密，不能使用明文的用户标识 |
| http-only | 不能通过JS访问Cookie，减少XSS攻击 |
| secure | 只能在协议为HTTPS的请求中携带 |
| same-site | 规定浏览器不能在跨域请求中携带Cookie，减少CSRF攻击 |

### 4.Service Worker
+ Service Worker 是运行在浏览器背后的独立线程，一般可以用来实现缓存功能。使用 Service Worker的话，传输协议必须为 HTTPS。因为 Service Worker 中涉及到请求拦截，所以必须使用 HTTPS 协议来保障安全。
+ Service Worker 实现缓存功能一般分为三个步骤：首先需要先注册 Service Worker，然后监听到 install 事件以后就可以缓存需要的文件，那么在下次用户访问的时候就可以通过拦截请求的方式查询是否存在缓存，存在缓存的话就可以直接读取缓存文件，否则就去请求数据。
```javascript
// index.js
if (navigator.serviceWorker) {
  navigator.serviceWorker
    .register('sw.js')
    .then(function(registration) {
      console.log('service worker 注册成功')
    })
    .catch(function(err) {
      console.log('servcie worker 注册失败')
    })
}
// sw.js
// 监听 `install` 事件，回调中缓存所需文件
self.addEventListener('install', e => {
  e.waitUntil(
    caches.open('my-cache').then(function(cache) {
      return cache.addAll(['./index.html', './index.js'])
    })
  )
})
// 拦截所有请求事件
// 如果缓存中已经有请求的数据就直接用缓存，否则去请求数据
self.addEventListener('fetch', e => {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      if (response) {
        return response
      }
      console.log('fetch source')
    })
  )
})
```

### 5. 强缓存和协商缓存
+ **强缓存** 可以通过设置两种 HTTP Header 实现：Expires 和 Cache-Control 。强缓存表示在缓存期间不需要请求，state code 为 200。
1. Expires 是 HTTP/1 的产物，表示资源会在 Wed, 22 Oct 2018 08:41:00 GMT 后过期，需要再次请求。并且 Expires 受限于本地时间，如果修改了本地时间，可能会造成缓存失效。
2. Cache-Control 出现于 HTTP/1.1，优先级高于 Expires 。该属性值表示资源会在 30 秒后过期，需要再次请求。

| 指令 | 作用 |
| ------ | ------ |
| public | 表示响应可以被客户端和代理服务器缓存 |
| private | 表示响应只可以被客户端缓存 |
| max-age=30 | 缓存30秒后就过期，需要重新请求 |
| s-maxage=30 | 覆盖max-age,作用一样，只在代理服务器中生效 |
| no-store | 不缓存任何响应 |
| no-cache | 资源被缓存，但是立即失效，下次会发起请求验证资源是否过期 |
| max-stale=30 | 30秒内，即使缓存过期，也使用该缓存 |
| min-fresh=30 | 希望在30秒内获取最新的响应 |

+ **协商缓存** 如果缓存过期了，就需要发起请求验证资源是否有更新。协商缓存可以通过设置两种 HTTP Header 实现：Last-Modified 和 ETag 。当浏览器发起请求验证资源时，如果资源没有做改变，那么服务端就会返回 304 状态码，并且更新浏览器缓存有效期。
1. Last-Modified 表示本地文件最后修改日期，If-Modified-Since 会将 Last-Modified 的值发送给服务器，询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来，否则返回 304 状态码。但是 Last-Modified 存在一些弊端：1.如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源；2.因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源
2. ETag 类似于文件指纹，If-None-Match 会将当前 ETag 发送给服务器，询问该资源 ETag 是否变动，有变动的话就将新的资源发送回来。并且 ETag 优先级比 Last-Modified 高。
+ **启发式缓存** 如果什么缓存策略都没设置，那么浏览器会怎么处理？对于这种情况，浏览器会采用一个启发式的算法，通常会取响应头中的 Date 减去 Last-Modified 值的 10% 作为缓存时间。

### 实际场景应用缓存策略举例
+ **频繁变动的资源** 对于频繁变动的资源，首先需要使用 Cache-Control: no-cache 使浏览器每次都请求服务器，然后配合 ETag 或者 Last-Modified 来验证资源是否有效。这样的做法虽然不能节省请求数量，但是能显著减少响应数据大小。
+ **代码文件** 这里特指除了 HTML 外的代码文件，因为 HTML 文件一般不缓存或者缓存时间很短。一般来说，现在都会使用工具来打包代码，那么我们就可以对文件名进行哈希处理，只有当代码修改后才会生成新的文件名。基于此，我们就可以给代码文件设置缓存有效期一年 Cache-Control: max-age=31536000，这样只有当 HTML 文件中引入的文件名发生了改变才会去下载最新的代码文件，否则就一直使用缓存。

### 重绘（Repaint）和回流（Reflow）是啥？如何优化？
+ 重绘是当节点需要更改外观而不会影响布局的，比如改变 color 就叫称为重绘。
+ 回流是布局或者几何属性需要改变就称为回流。
回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变父节点里的子节点很可能会导致父节点的一系列回流。
+ 优化方案：
1. 使用 transform 替代 top
2. 使用 visibility 替换 display: none ，因为前者只会引起重绘，后者会引发回流（改变了布局）
3. 不要把节点的属性值放在一个循环里当成循环里的变量
4. 不要使用 table 布局，可能很小的一个小改动会造成整个 table 的重新布局
5. 动画实现的速度的选择，动画速度越快，回流次数越多，也可以选择使用requestAnimationFrame
6. CSS 选择符从右往左匹配查找，避免节点层级过多
7. 将频繁重绘或者回流的节点设置为图层，图层能够阻止该节点的渲染行为影响别的节点。比如对于 video 标签来说，浏览器会自动将该节点变为图层。
