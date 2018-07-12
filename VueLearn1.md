# Vue学习(1)

## 一、介绍

Vue是一套用于构建用户界面的渐进式框架。

[官方文档](https://cn.vuejs.org/)

## 二、MVVM的介绍

MVVM，一种更好的UI模式解决方案

### 2.1 MVC
* M: Model 数据模型（专门用来操作数据，数据的CRUD）
* V：View 视图（对于前端来说，就是页面）
* C：Controller 控制器（是视图和数据模型沟通的桥梁，用于处理业务逻辑）

### 2.2 MVVM
* MVVM ===> M / V / VM
* M：model数据模型
* V：view视图
* VM：ViewModel 视图模型

### 2.3 优势对比
* MVC模式，将应用程序划分为三大部分，实现了职责分离
* 在前端中经常要通过 JS代码 来进行一些逻辑操作，最终还要把这些逻辑操作的结果现在页面中。也就是需要频繁的操作DOM
* MVVM通过**数据双向绑定**让数据自动地双向同步
    * V（修改数据） -> M
    * M（修改数据） -> V
    * 数据是核心
* Vue这种MVVM模式的框架，不推荐开发人员手动操作DOM

## 三、快速开始

### 3.1 不使用脚手架

#### 3.1.1 起步

新建 *index.html*
```html
<html>
    <head>
        <!-- 引入Vue的开发环境版本，包含了有帮助的命令行警告 -->
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    </head>
    <body>
        <!-- 指定vue管理内容区域，需要通过vue展示的内容都要放到找个元素中。通常我们也把它叫做边界，数据只在边界内部解析-->
        <div id="app">
            {{ message }}
        </div>
        <script src="index.js"></script>
    </body>
</html>
```
新建 *index.js*
```js
var app = new Vue({
  // el：提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标
  el: '#app',
  // Vue 实例的数据对象，用于给 View 提供数据
  data: {
    message: 'Hello Vue!'
  }
})
```
#### 3.1.2 Vue实例
* 先在data中声明数据，再使用数据
* 可以通过 vm.$data 访问到data中的所有属性，或者 vm.msg
```js
var vm = new Vue({
  data: {
    msg: '大家好，...'
  }
})

vm.$data.msg === vm.msg // true
```

#### 3.1.3 数据绑定
* 最常用的方式：Mustache(插值语法)，也就是 {{}} 语法
* 解释：{{}}从数据对象data中获取数据
* 说明：数据对象的属性值发生了改变，插值处的内容都会更新
* 说明：{{}}中只能出现JavaScript表达式 而不能解析js语句
* 注意：Mustache 语法不能作用在 HTML 元素的属性上
```html
<h1>Hello, {{ msg }}.</h1>
<p>{{ 1 + 2 }}</p>
<p>{{ isOk ? 'yes': 'no' }}</p>

<!-- 正确 -->
<div v-bind:title="msg"></div>
<!-- 简写 -->
<div :title="msg"></div>
<!-- 错误 -->
<h1 title="{{ msg }}"></h1>

```
更多细节以及demo可查看:
[官方文档-起步](https://cn.vuejs.org/v2/guide/index.html)

### 3.2 使用脚手架

#### 3.2.1 配置安装脚手架

##### 3.2.1.1 安装淘宝镜像 cnpm

`npm install -g cnpm --registry=https://registry.npm.taobao.org`

##### 3.2.1.2 全局安装 vue-cli 2.x

到放项目的目录下shift+右键打开命令行窗口

`cnpm i -g @vue/cli-init`

##### 3.2.1.3 创建一个基于 webpack 模板的新项目

`vue init webpack vuecli`

（一直回车，只有vue-router选择y，其他均选择n）

##### 3.2.1.4 安装依赖

`cd vuecli`

`cnpm i`

##### 3.2.1.5 运行项目

`npm run dev`

更多安装细节可查看:
[搭建Vue脚手架](https://github.com/vectorzero/notes/tree/master/Vue.md)

#### 3.2.2 vue文件代码结构
```html
<!-- html代码，由一个template标签包住 -->
<template>
    <div class="wrap">
        <p>Hello Vue!</p>
        <button @click="showTime"></button>
        <BtnCurdMethod></BtnCurdMethod>
    </div>
</template>

<!-- javascript代码 -->
<script>
    import BtnCurdMethod from '@/components/BtnCurdMethod'
    export default {
        // 和数据相关的都挂载到data上
        data() {
            return {
                msg: '你好'
            }
        },
        props: {
            'xxArray': {
                type: Array,
                default: function () {
                    return []
                }
            },
            'xxObject': {
                type: Object,
                default: function() {
                    return {}
                }
            },
        },
        components: {
            BtnCurdMethod
        },
        computed() {

        },
        watch() {

        },
        // 和方法相关的都挂载到methods上
        methods: {
            init() {
                console.log('初始化')
            },
            showTime() {
                console.log(this.msg)
            },
            //...
        },
        //生命周期
        created() {
            this.init();
        },
        mounted() {
            
        },
        //...
    }
</script>

<!-- css/less/sass代码 -->
<style lang="less" scoped>
    .wrap {
        background: black;
        color: yellow;
    }
</style>
```

#### 3.2.3 知识点

* `export default` & `import`

点击查看:
[ECMAScript6 Module 的语法](http://es6.ruanyifeng.com/#docs/module)

* 数据 - `data`
> 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。
> 当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

更多细节以及demo可查看:[官方文档-data](https://cn.vuejs.org/v2/api/#data)

* 父组件数据 - `props`

    * 方式：通过子组件props属性来传递数据
    * 注意：属性的值必须在组件中通过props属性显示指定，否则，不会生效
    * 说明：传递过来的props属性的用法与data属性的用法相同
> props 可以是数组或对象，用于接收来自父组件的数据。
> props 可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义校验和设置默认值。

更多细节以及demo可查看:
[官方文档-props](https://cn.vuejs.org/v2/api/#props)

* 组件 - `components`

更多细节以及demo可查看:[官方文档-components](https://cn.vuejs.org/v2/guide/components.html
)

* 监视数据变化 - `watch`

    * 概述：watch是一个对象，键是需要观察的表达式，值是对应回调函数
    * 作用：当表达式的值发生变化后，会调用对应的回调函数完成响应的监视操作
```js
new Vue({
  data: { a: 1, b: { age: 10 } },
  watch: {
    a: function(val, oldVal) {
      // val 表示当前值
      // oldVal 表示旧值
      console.log('当前值为：' + val, '旧值为：' + oldVal)
    },

    // 监听对象属性的变化
    b: {
      handler: function (val, oldVal) { /* ... */ },
      // deep : true表示是否监听对象内部属性值的变化 
      deep: true
    },

    // 只监视user对象中age属性的变化
    'user.age': function (val, oldVal) {

    },
  }
})
```

更多细节以及demo可查看:
[官方文档-watch](https://cn.vuejs.org/v2/api/#watch)
     
* 计算属性 - `computed`

    * 说明：计算属性是基于它们的依赖进行缓存的，只有在它的依赖发生改变时才会重新求值
    * 注意：Mustache语法（{{}}）中不要放入太多的逻辑，否则会让模板过重、难以理解和维护
    * 注意：computed中的属性不能与data中的属性同名，否则会报错
```js
var vm = new Vue({
  el: '#app',
  data: {
    firstname: 'jack',
    lastname: 'rose'
  },
  computed: {
    fullname() {
      return this.firstname + '.' + this.lastname
    }
  }
})
```

更多细节以及demo可查看:
[官方文档-computed](https://cn.vuejs.org/v2/api/#computed)

* 生命周期

一个组件从开始到最后消亡所经历的各种状态，就是一个组件的生命周期。

生命周期钩子函数：

*beforeCreate()*、*created()*、*beforeMounted()*、*mounted()*、*beforeUpdated()*、*updated()*、*beforeDestroy()*、*destroyed()*

生命周期钩子函数的定义：从组件被创建，到组件挂载到页面上运行，再到页面关闭组件被卸载，这三个阶段总是伴随着组件各种各样的事件，这些事件，统称为组件的生命周期函数！

> 不要在选项属性或回调上使用箭头函数，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致 Uncaught TypeError: Cannot read property of undefined 或 Uncaught TypeError: this.myMethod is not a function 之类的错误。

![avatar](https://cn.vuejs.org/images/lifecycle.png)

`beforeCreate()`

说明：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用

注意：此时，无法获取 data中的数据、methods中的方法

`created()`

注意：这是一个常用的生命周期，可以调用methods中的方法、改变data中的数据

使用场景：发送请求获取数据

`beforeMounted()`

说明：在挂载开始之前被调用

`mounted()`

说明：此时，vue实例已经挂载到页面中，可以获取到el中的DOM元素，进行DOM操作

`beforeUpdated()`

说明：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程

注意：此处获取的数据是更新后的数据，但是获取页面中的DOM元素是更新之前的

`updated()`

说明：组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作

`beforeDestroy()`

说明：实例销毁之前调用。在这一步，实例仍然完全可用

使用场景：实例销毁之前，执行清理任务，比如：清除定时器等

`destroyed()`

说明：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

更多细节以及demo可查看:[官方文档-选项 / 生命周期钩子](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)



