## vue面试题
参考：[Vue面试中，经常会被问到的面试题/Vue知识点整理](https://segmentfault.com/a/1190000016344599)

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

### 5.vue的父子组件如何通信的
+ 父组件是通过props属性给子组件通信
+ 子组件通过触发事件来改变父组件的数据（this.$emit(method,data))。父组件通过监听子组件触发的事件，来调用方法，接收数据。
+ 兄弟组件的通信，采用观察者模式。新建一个第三方实例。
```JavaScript
//组件他哥
<div @click="ge"></div>
methods: {
    ge() {
        vm.$emit('blur','sichaoyun'); //触发事件
    }
}
//组件小弟
vm.$on('blur', (arg) => {
        this.test= arg; // 接收
});
```

### 6.Vue的路由实现：hash模式 和 history模式
+ **hash模式：** 在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；
+ 特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 http://www.xxx.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。
+ **history模式：** history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。
+ history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。

### 7.vue-cli如何新增自定义指令？
```JavaScript
//创建局部指令
var app = new Vue({
    el: '#app',
    data: {    
    },
    // 创建指令(可以多个)
    directives: {
        // 指令名称
        dir1: {
            inserted(el) {
                el.style.width = '200px';
            }}}})
//创建全局指令
Vue.directive('dir2', {
    inserted(el) {
        console.log(el);
    }
})
//指令的使用
<div v-dir1></div>
```

### 8.vue如何自定义一个过滤器？
```JavaScript
<div id="app">
     <input type="text" v-model="msg" />
     {{msg| capitalize }}
</div>

var vm=new Vue({
    el:"#app",
    data:{
        msg:''
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
})
```

### 9.对keep-alive 的了解？
+ keep-alive是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
+ 在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 。

### 10. vue中 key 值的作用？
+ 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟DOM。

### 11.什么是vue的计算属性？
在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。好处：①使得数据处理结构清晰；②依赖于数据，数据更新，处理结果自动更新；③计算属性内部this指向vm实例；④在template调用时，直接写计算属性名即可；⑤常用的是getter方法，获取数据，也可以使用set方法改变数据；⑥相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。

### 12. vuex原理
+ vuex的store有State、 Getter、Mutation 、Action、 Module五种属性；
+ **state** 为单一状态树，在state中需要定义我们所需要管理的数组、对象、字符串等等
+ **getters** 类似vue的计算属性，主要用来过滤一些数据。
+ **mutation** 更改store中state状态的唯一方法就是提交mutation，store.commit。
+ **action** actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。
+ **module** module其实只是解决了当state中很复杂臃肿的时候，module可以将store分割成模块，每个模块中拥有自己的state、mutation、action和getter。

### 13. vue数据双向绑定
```JavaScript
<body>
    <div id="app">
    <input type="text" id="txt">
    <p id="show"></p>
</div>
</body>
<script type="text/javascript">
    var obj = {}
    Object.defineProperty(obj, 'txt', {
        get: function () {
            return obj
        },
        set: function (newValue) {
            document.getElementById('txt').value = newValue
            document.getElementById('show').innerHTML = newValue
        }
    })
    document.addEventListener('keyup', function (e) {
        obj.txt = e.target.value
    })
</script>
```

### 14. 父子组件如何通信，兄弟组件如何通信
1. props/$emit+v-on: 通过props将数据自上而下传递，而通过$emit和v-on来向上传递信息。
2. EventBus: 通过EventBus进行信息的发布与订阅
3. vuex: 是全局数据管理库，可以通过vuex管理全局的数据流
4. $attrs/$listeners: Vue2.4中加入的$attrs/$listeners可以进行跨级的组件通信
5. provide/inject：以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效，这成为了跨组件通信的基础

### 15. Proxy与Object.defineProperty的优劣对比?
+ **Object.defineProperty的缺点**
1. 无法监听数组变化。Vue的文档提到了Vue是可以检测到数组变化的，但是只有以下八种方法,vm.items[indexOfItem] = newValue这种是无法检测的。
2. 只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历，如果属性值也是对象那么需要深度遍历,显然能劫持一个完整的对象是更好的选择。。
+ **Proxy的优点：**
1. Proxy可以直接监听对象而非属性
2. Proxy可以直接监听数组的变化
3. Proxy有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的
4. Proxy返回的是一个新对象,我们可以只操作新的对象达到目的,而Object.defineProperty只能遍历对象属性直接修改
5. Proxy作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利