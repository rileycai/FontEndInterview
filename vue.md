## vue面试题

### 1.vue 动态路由加载
在vue-router对象中首先初始化公共路由，比如（404，login）等，然后在用户登陆成功，根据用户的角色信息，获取对应权限菜单信息menuList，并将后台返回的menuList转换成我们需要的router数据结构，然后通过vue-router2.2新添的router.addRouter(routes)方法，同时我们可以将转后的路由信息保存于vuex，这样我们可以在我们的SideBar组件中获取我们的全部路由信息，并且渲染我们的左侧菜单栏，让动态路由实现。

+ 登录：当用户填写完账号和密码后向服务端验证是否正确，验证通过之后，服务端会返回一个token，拿到token之后（我会将这个token存贮到cookie中，保证刷新页面后能记住用户登录状态），前端会根据token再去拉取一个 user_info 的接口来获取用户的详细信息（如用户权限，用户名等等信息）。
+ 权限验证：通过token获取用户对应的 role，动态根据用户的 role 算出其对应有权限的路由，通过router.addRoutes 动态挂载这些路由。


参考： [vue后台管理之动态加载路由的方法](https://www.jb51.net/article/145531.htm)

### 2. webpack中alias配置
作用:设置别名是为了让后续引用的地方减少路径的复杂度
```javascript
//index.vue 里，正常引用 A 组件：
import A from '../../components/a.vue'
//如果设置了 alias 后。
alias: {
  'vue$': 'vue/dist/vue.esm.js',
  '@': resolve('src')
}
import A from '@/components/a.vue'
```
### 3.vue如何实现双向绑定
+ vue数据双向绑定是通过 **数据劫持** 结合 **发布者-订阅者模式** 的方式来实现的.
+ vue是通过Object.defineProperty()来实现数据劫持的。它可以来控制一个对象属性的一些特有操作，比如读写权、是否可以枚举.有get和set两个方法。
+ 实现mvvm主要包含两个方面，数据变化更新视图，视图变化更新数据。视图更新数据其实可以通过事件监听即可，关键在于数据变化更新视图，通过Object.defineProperty( )对属性设置一个set函数，当数据改变了就会来触发这个函数，所以我们只要将一些需要更新的方法放在这里面就可以实现data更新view了。
+ 实现数据的双向绑定：
1. 实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
2. 实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
3. 实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。

### 4. vue的生命周期
+ **beforecreated**  el、data、message未被初始化，此时可用于loading
+ **created** 完成了data、message的初始化
+ **beforeMount** 完成了el和data的初始化 ,但是el还是 {{message}}，这里就是应用的 Virtual DOM（虚拟Dom）技术，先把坑占住了。
+ **mounted** 完成挂载，渲染值。
+ **beforeUpdate** 可以监听到data的变化但是view层没有被重新渲染，view层的数据没有变化
+ **updated** 等到updated的时候 view层才被重新渲染，数据更新。
+ **beforeDestroy** 钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。
+ **destroyed** 钩子函数在Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
