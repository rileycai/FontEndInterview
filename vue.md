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
