## vue面试题

### 1.vue 动态路由加载
在vue-router对象中首先初始化公共路由，比如（404，login）等，然后在用户登陆成功，根据用户的角色信息，获取对应权限菜单信息menuList，并将后台返回的menuList转换成我们需要的router数据结构，然后通过vue-router2.2新添的router.addRouter(routes)方法，同时我们可以将转后的路由信息保存于vuex，这样我们可以在我们的SideBar组件中获取我们的全部路由信息，并且渲染我们的左侧菜单栏，让动态路由实现。

参考： [vue后台管理之动态加载路由的方法](https://www.jb51.net/article/145531.htm)
