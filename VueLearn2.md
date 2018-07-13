### 4.4 监视数据变化 - `watch`

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

更多细节以及demo可查看：[官方文档-watch](https://cn.vuejs.org/v2/api/#watch)
     
### 4.5 计算属性 - `computed`

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

更多细节以及demo可查看：[官方文档-computed](https://cn.vuejs.org/v2/api/#computed)

### 4.6 生命周期

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

更多细节以及demo可查看：[官方文档-选项 / 生命周期钩子](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)

### 4.7 指令
* 解释：指令 (Directives) 是带有 v- 前缀的特殊属性
* 作用：当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM

#### 4.7.1 [模板语法](https://cn.vuejs.org/v2/guide/syntax.html)

* `v-text`
    * 解释：更新DOM对象的 textContent
```html
<h1 v-text="msg"></h1>
<!-- 等同于 -->
<h1>{{ msg }}</h1>
```

* `v-html`
    * 解释：更新DOM对象的 innerHTML
```html
<h1 v-html="msg"></h1>
```
   
* `v-bind`
    * 作用：当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM
    * 语法：v-bind:title="msg"
    * 简写：:title="msg"
```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

#### 4.7.2 [条件渲染](https://cn.vuejs.org/v2/guide/conditional.html)

* `v-if`,`v-show`
    * v-if：根据表达式的值的真假条件，销毁或重建元素
    * v-show：根据表达式之真假值，切换元素的display属性
```html
<p v-show="isShow">这个元素展示出来了吗？？？</p>
<p v-if="isShow">这个元素，在HTML结构中吗？？？</p>
```

#### 4.7.3 [列表渲染](https://cn.vuejs.org/v2/guide/list.html)

* `v-for`
    * 作用：基于源数据多次渲染元素或模板块
```html
<!-- 基础用法 -->
<div v-for="item in items">
  {{ item.text }}
</div>

<!-- item 为当前项，index 为索引 -->
<p v-for="(item, index) in list">{{item}} -- {{index}}</p>
<!-- item 为值，key 为键，index 为索引 -->
<p v-for="(item, key, index) in obj">{{item}} -- {{key}}</p>
<p v-for="item in 10">{{item}}</p>
```

#### 4.7.4 [事件处理](https://cn.vuejs.org/v2/guide/events.html)

* `v-on`
    * 作用：绑定事件
    * 语法：v-on:click="say" or v-on:click="say('参数', $event)"
    * 简写：@click="say"
    * 说明：绑定的事件定义在methods
```html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

* 事件修饰符
    * `.stop` 阻止冒泡，调用 event.stopPropagation()
    * `.prevent` 阻止默认行为，调用 event.preventDefault()
    * `.capture` 添加事件侦听器时使用事件捕获模式
    * `.self` 只当事件在该元素本身（比如不是子元素）触发时，才会触发事件
    * `.once` 事件只触发一次

* `v-model`
    * 作用：在表单元素上创建双向数据绑定
    * 说明：监听用户的输入事件以更新数据
    * 案例：计算器
```html
<input type="text" v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

#### 4.7.5 [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

* 作用：进行DOM操作
* 使用场景：对纯 DOM 元素进行底层操作，比如：文本框获得焦点
* vue 自定义指令用法实例

全局自定义指令
```js
// 第一个参数：指令名称
// 第二个参数：配置对象，指定指令的钩子函数
Vue.directive('directiveName', {
  // bind中只能对元素自身进行DOM操作，而无法对父级元素操作
  // 只调用一次 指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
  bind( el，binding, vnode ) {
    // 参数详解
    // el：指令所绑定的元素，可以用来直接操作 DOM 。
    // binding：一个对象，包含以下属性：
      // name：指令名，不包括 v- 前缀。
      // value：指令的绑定值，等号后面的值 。
      // oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
      // expression：字符串形式的指令表达式 等号后面的字符串 形式
      // arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
      // modifiers：指令修饰符。例如：v-directive.foo.bar中，修饰符对象为 { foo: true, bar: true }。
    // vnode：Vue 编译生成的虚拟节点。。
    // oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
  },
  // inserted这个钩子函数调用的时候，当前元素已经插入页面中了，也就是说可以获取到父级节点了
  inserted (  el，binding, vnode ) {},
  //  DOM重新渲染前
  update(el，binding, vnode,oldVnode) {},
  // DOM重新渲染后
  componentUpdated ( el，binding, vnode,oldVnode ) {},
  // 只调用一次，指令与元素解绑时调用
  unbind ( el ) {
    // 指令所在的元素在页面中消失，触发
  }
})
// 简写 如果你想在 bind 和 update 时触发相同行为，而不关心其它的钩子:
Vue.directive('自定义指令名', function( el, binding ) {})
// 例：
Vue.directive('color', function(el, binding) {
  el.style.color = binging.value
})
```

局部自定义指令
```js
var vm = new Vue({
  el : "#app",
  directives: {
    directiveName: { }
  }
})
```

更多细节以及demo可查看：[官方文档-指令](https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4)

### 4.8 样式处理 - class和style

```js
    data() {
        return {
            isActive: true,
            errorClass： 'error-class',
            activeColor: 'yellow',
            fontSize: 12,
            baseStyles: {
                //css样式
            },
            overridingStyles: {
                //css样式
            },
        }
    }
```

```html
<!-- class -->
<div v-bind:class="{ active: isActive }"></div>
<!-- isActive为true,等同于👇 -->
<div class="active"></div>

<div :class="['active', 'text-danger']"></div>
<!-- 等同于👇 -->
<div class="active text-danger"></div>

<div v-bind:class="[{ active: isActive }, errorClass]"></div>
<!-- isActive为true,errorClass的值为'error-class',等同于👇 -->
<div class="active error-class"></div>

<!-- style -->
<div :style="{ color: activeColor, 'font-size': fontSize + 'px' }"></div>
<!-- activeColor为'yellow',fontSize为12,等同于👇 -->
<div style="color: yellow; font-size: 12px "></div>

<!-- 2 将多个 样式对象 应用到一个元素上-->
<!-- baseStyles 和 overridingStyles 都是data中定义的对象 -->
<div :style="[baseStyles, overridingStyles]"></div>
```

更多细节以及demo可查看：[官方文档-Class 与 Style 绑定](https://cn.vuejs.org/v2/guide/class-and-style.html)

