# Vue学习(1)

## 一、介绍

Vue是一套用于构建用户界面的渐进式框架。

点击查看：[官方文档](https://cn.vuejs.org/)

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
更多细节以及demo可查看：[官方文档-起步](https://cn.vuejs.org/v2/guide/index.html)

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

更多安装细节可查看：[搭建Vue脚手架](https://github.com/vectorzero/notes/tree/master/Vue.md)

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

## 四、知识点

### 4.1 `export default` & `import`

点击查看：[ECMAScript6 Module 的语法](http://es6.ruanyifeng.com/#docs/module)

### 4.2 数据 - `data`
> 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。
> 当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

更多细节以及demo可查看:[官方文档-data](https://cn.vuejs.org/v2/api/#data)


### 4.3 组件 - `components`
>组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树

#### 子组件 *child.vue*
```html
<template>
    <!-- template 只能有一个根元素 -->
    <div>
        <p>A custom component!</p>
        <p>{{ msg }}</p>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                msg: 'test'
            }
        }
    }
</script>
```

#### 父组件 *parent.vue*
```html
<template>
    <div>
        <h2>组件</h2>
        <child></child>
        <!-- <child/>等同于👇 -->
        <!--
            <div>
                <p>A custom component!</p>
                <p>test</p>
            </div>
        -->
    </div>
</template>
<script>
    //引入子组件
    import child from '@/components/child.vue'
    export default {
        data() {
            return {
                //...
            }
        },
        //注册组件
        components: ['child']
    }
</script>
```

更多细节以及demo可查看: [官方文档-components](https://cn.vuejs.org/v2/guide/components.html)

#### 父组件数据 - `props`

 * 方式：通过子组件props属性来传递数据
 * 注意：属性的值必须在组件中通过props属性显示指定，否则，不会生效
 * 说明：传递过来的props属性的用法与data属性的用法相同

> props 可以是数组或对象，用于接收来自父组件的数据。
> props 可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义校验和设置默认值。

更多细节以及demo可查看：[官方文档-props](https://cn.vuejs.org/v2/api/#props)

#### 组件通讯 todo
