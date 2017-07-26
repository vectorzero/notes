## 下载nodejs （win64位，版本最好为6.x）
- [下载node.js](http://nodejs.cn/download/)
- 安装nodejs

**（Win+R输入cmd打开命令行窗口）**

## 安装淘宝镜像 cnpm
`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 全局安装 vue-cli
`cnpm i vue-cli -g`

**（关闭cmd窗口，到放项目的目录下shift+右键打开命令行窗口）**

## 创建一个基于 webpack 模板的新项目
`vue init webpack vuecli`

**(`vue init webpack-simple vuecli`)**

**（一直回车，只有vue-router选择y，其他均选择n）**

## 安装依赖
`cd vuecli`

`cnpm i`

## 运行项目
`npm run dev`

## 安装elementUI
`cnpm i element-ui -S`

## 引入elementUI
在**main.js**中添入以下代码
```javascript
import ElementUI from 'element-ui' 
import 'element-ui/lib/theme-default/index.css'
Vue.use(ElementUI)
```

## 安装 font-awesome
`cnpm i font-awesome -S`

## 引入font-awesome
在**main.js**里面添加以下代码
```javascript
import 'font-awesome/css/font-awesome.css'
```

#### 使用代码：
```html
<i class="fa fa-fw fa-weibo"></i>
```

## 安装axios
`cnpm i axios -S`

## 引入axios
在 **main.js** 文件中添加以下代码
```javascript
import axios from 'axios'
Vue.prototype.$http = axios
```

#### 使用代码：
```javascript
this.$http(‘http://localhost:8080/static/data.json’)
  .then((response)=>{
    let data = response.data;
  })
  .catch((response)=>{
    console.log(response)
  })
```

## 安装vuex
`cnpm i vuex -S`

## 引入vuex
在**mian.js**中添加以下代码：
```javascript
import Vuex from 'vuex'
Vue.use(Vuex)
```

#### 使用代码：
```javascript
const store = new Vuex.Store({
  // 存储状态值
  state: {
    count:1
  },
  // 状态值的改变方法,操作状态值
  // 提交mutations是更改Vuex状态的唯一方法
  mutations: {
    increment(state){
      state.count++
    }
  },
  // 在store中定义getters（可以认为是store的计算属性）。Getters接收state作为其第一个函数
  getters: {
    
  },
  actions: { 
    
  }
})

new Vue({
  el: '#app',
  router,
  store,
  template: '<App/>',
  components: { App }
})
```

## 安装less
`cnpm i less less-loader -S`

## 引入less
在**webpack.base.conf.js**里面添加以下代码
```javascript
module: {
    rules: [
        {
          test: /\.less$/,
          loader: "style-loader!css-loader!less-loader"
        }
    ]
}
```

#### 使用代码：
```html
<style lang='less'>
.nav {
      a {
        text-decoration: none;
        margin: auto 20px;
      }
}
</style>
```

## 安装vux
`cnpm i vux -S`
`cnpm i vux-loader`

## 引入vux
在**webpack.base.conf.js**里面添加/修改以下代码
```javascript
const vuxLoader = require(‘vux-loader’)
//module.exports = {
let webpackConfig = {
  entry: {
    //....
  }
    //...
}
module.exports = vuxLoader.merge(webpackConfig, {
    options: {},
    plugins: [
        {
            name: 'vux-ui'
        }
    ]
})
```

#### 使用代码：
```html
<group>
  <cell title="title" value="value"></cell>
</group>
```
```javascript
import { Group, Cell } from 'vux'
export default {
  components: {
    Group,
    Cell
  }
}
```

## 引入amap
在**mian.js**中添加以下代码：
```javascript
import AMap from 'vue-amap'
Vue.use(AMap)

AMap.initAMapApiLoader({
  // 申请的高德key
  key: 'YOUR_KEY',
  // 插件集合
  plugin: ['']
});
```

#### 使用代码：
```html
<el-amap vid="amapDemo"></el-amap>
```