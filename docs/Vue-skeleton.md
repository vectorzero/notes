# Vue骨架屏

1. 安装`prerender-spa-plugin`
`npm install prerender-spa-plugin -S`

2. 配置*webpack*

*build/webpack.prod.conf.js*
```js
const PrerenderSpaPlugin = require('prerender-spa-plugin')

module.exports = {
  // ...
  plugins: [
    new PrerenderSpaPlugin(
      // Absolute path to compiled SPA
      path.join(__dirname, '../dist'),
      // List of routes to prerender
      ['/']
    )
  ]
} 
```

3. 新建骨架屏文件

*src/skeleton.vue*
```html
<template>
  <div>
    <!-- 骨架屏代码 -->
  </div>
</template>
```

4. 修改*App.vue*文件

```html
<!-- 当初次进入页面的时候我们需要显示骨架屏，数据加载完，我们需要移除骨架屏 -->
 <template>
  <div id="app">
    <mainSkeleton v-if="!init"></mainSkeleton>
    <div v-else>
      <div class="body"></div>
    </div>
  </div>
</template>
<script>
  import mainSkeleton from './skeleton.vue'
  export default {
    name: 'app',
    data () {
      return {
        init: false
      }
    },
    mounted () {
      //  这里模拟数据请求
      setTimeout(() => {
        this.init = true
      }, 250)
    },
    components: {
      mainSkeleton
    }
  }
</script>
```

5. 为项目初次加载渲染时添加loading

*index.html*
```html
<div id="app">
  <div class="loading"></div>
</div>
```