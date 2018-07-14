# 一、SPA

单页Web应用（single page application，SPA），就是只有一个Web页面的应用，是加载单个HTML页面，并在用户与应用程序交互时动态更新该页面的Web应用程序。

* 单页面应用程序：
    * 只有第一次会加载页面, 以后的每次请求, 仅仅是获取必要的数据.然后, 由页面中js解析获取的数据, 展示在页面中
* 传统多页面应用程序：
    * 对于传统的多页面应用程序来说, 每次请求服务器返回的都是一个完整的页面

## 优势

1. 减少了请求体积，加快页面响应速度，降低了对服务器的压力
2. 更好的用户体验，让用户在web app感受native app的流畅

## 实现思路和技术点

1. ajax
2. 锚点的使用（window.location.hash #）
3. hashchange 事件 window.addEventListener("hashchange",function () {})
4. 监听锚点值变化的事件，根据不同的锚点值，请求相应的数据
5. 原本用作页面内部进行跳转，定位并展示相应的内容

# 二、路由

路由即浏览器中的哈希值（# hash）与展示视图内容（template）之间的对应规则

vue中的路由是：hash 和 component的对应关系

在 Web app 中，通过一个页面来展示和管理整个应用的功能。

SPA往往是功能复杂的应用，为了有效管理所有视图内容，前端路由应运而生！

简单来说，路由就是一套映射规则（一对一的对应规则），由开发人员制定规则。

当URL中的哈希值（# hash）发生改变后，路由会根据制定好的规则，展示对应的视图内容.

## 基本使用

### 安装：`npm i -S vue-router`
```html
<div id="app">
    <!-- 路由入口 指定跳转到只定入口 -->
    <router-link to="/home">首页</router-link>
    <router-link to="/login">登录</router-link>

    <!-- 路由出口：用来展示匹配路由视图内容 -->
    <router-view></router-view>
</div>

<!-- 导入 vue.js -->
<script src="./vue.js"></script>
<!-- 导入 路由文件 -->
<script src="./node_modules/vue-router/dist/vue-router.js"></script>
<script>
    // 创建两个组件
    const Home = Vue.component('home', {
        template: '<h1>这是 Home 组件</h1>'
    })
    const Login = Vue.component('login', {
        template: '<h1>这是 Login 组件</h1>'
    })

    // 创建路由对象
    const router = new VueRouter({
    routes: [
            // 路径和组件一一对应
            { path: '/home', component: Home },
            { path: '/login', component: Login }
        ]
    })

    var vm = new Vue({
        el: '#app',
        // 将路由实例挂载到vue实例
        router
    })
</script>
```

### 重定向
```js
// 将path 重定向到 redirect
{ path: '/', redirect: '/home' }
```

## 路由其他配置
### 路由导航高亮
* 说明：当前匹配的导航链接，会自动添加router-link-exact-active router-link-active类

* 配置：linkActiveClass

### 匹配路由模式

#### 配置：mode

```js
new Router({
  routers:[],
  mode: "hash", //默认hash | history 可以达到隐藏地址栏hash值 | abstract，如果发现没有浏览器的 API 则强制进入
  linkActiveClass : "now" //当前匹配的导航链接将被自动添加now类
})
```
### 路由参数

* 说明：我们经常需要把某种模式匹配到的所有路由，全都映射到同一个组件，此时，可以通过路由参数来处理
* 语法：/user/:id
* 使用：当匹配到一个路由时，参数值会被设置到 this.$route.params
* 其他：可以通过 $route.query 获取到 URL 中的查询参数 等

```html
    <!-- 方式一 -->
    <router-link to="/user/1001">如果你需要在模版中使用路由参数 可以这样 {{$router.params.id}}</router-link>
    <!-- 方式二 -->
    <router-link :to="{path:'/user',query:{name:'jack',age:18}}">用户 Rose</router-link>
    <script>
    // 路由
    var router = new Router({
      routers : [
        // 方式一 注意 只有/user/1001这种形式能被匹配 /user | /user/ | /user/1001/ 都不能被匹配
        // 将来通过$router.params获取参数返回 {id:1001}
        { path: '/user/:id', component: User }, 
        // 方式二
        { path: "user" , component: User}
      ]
    })
    // User组件：
    const User = {
      template: `<div>User {{ $route.params.id }}</div>`
    }
    </script>  
    <!-- 如果要子啊vue实例中获取路由参数 则使用this.$router.params 获取路由参数对象 -->
    <!--  {{$router.query}} 获取路由中的查询字符串 返回对象 -->
```

#### 配置：history 

TODO

## 嵌套路由 - 子路由

* 路由是可以嵌套的，即：路由中又包含子路由
* 规则：父组件中包含 router-view，在路由规则中使用 children 配置

```js
// 父组件：
const User = Vue.component('user', {
    template: `
    <div class="user">
        <h2>User Center</h2>
        <router-link to="/user/profile">个人资料</router-link>
        <router-link to="/user/posts">岗位</router-link>
        <!-- 子路由展示在此处 -->
        <router-view></router-view>
    </div>
    `
})

// 子组件
const UserProfile = {
    template: '<h3>个人资料：张三</h3>'
}
const UserPosts = {
    template: '<h3>岗位：FE</h3>'
}

// 路由
var router =new Router({
    routers : [
        { path: '/user', component: User,
            // 子路由配置：
            children: [
                {
                    // 当 /user/profile 匹配成功，
                    // UserProfile 会被渲染在 User 的 <router-view> 中
                    path: 'profile',
                    component: UserProfile
                },
                {
                    // 当 /user/posts 匹配成功
                    // UserPosts 会被渲染在 User 的 <router-view> 中
                    path: 'posts',
                    component: UserPosts
                }
            ]
        }
    ]
})
```
更多细节以及demo可查看:[官方文档-vue-router](https://router.vuejs.org/zh/)
