## 下载nodejs （win64位，版本最好为6.x以上）
- [下载node.js](http://nodejs.cn/download/)
- 安装nodejs

**（Win+R输入cmd打开命令行窗口）**

## 安装淘宝镜像 cnpm
`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 全局安装 vue-cli 2.x
关闭cmd窗口，到放项目的目录下shift+右键打开命令行窗口

`cnpm i -g @vue/cli-init`

## 创建一个基于 webpack 模板的新项目
`vue init webpack vuecli`

**（一直回车，只有vue-router选择y，其他均选择n）**

## 安装依赖
`cd vuecli`

`cnpm i`

## 运行项目
`npm run dev`

## 安装elementUI
`cnpm i element-ui --save-dev`

## 引入elementUI
在**main.js**中添入以下代码
```javascript
import ElementUI from 'element-ui' 
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI, { size: 'small' })
```
## 安装axios
`cnpm i axios --save-dev`

## 引入axios
在 **main.js** 文件中添加以下代码
```javascript
import axios from 'axios'
Vue.prototype.$http = axios
```

#### 使用代码：
```javascript
this.$http({method:'get',url:'http://localhost:8080/static/data.json',param:{}})
  .then((response)=>{
    console.log(response.data)
  })
  .catch((error)=>{
    console.log(error)
  })
```

## 安装vuex
`cnpm i vuex --save-dev`

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
`cnpm i less less-loader --save-dev`

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
## 在vue里面使用jsx语法：

`cnpm i babel-helper-vue-jsx-merge-props --save-dev`

`cnpm i babel-plugin-syntax-jsx --save-dev`

`cnpm i babel-plugin-transform-vue-jsx --save-dev`

在**babelrc**文件里面添加/修改以下代码：
```
{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins": ["transform-runtime","transform-vue-jsx"],
  "env": {
    "test": {
      "presets": ["env", "stage-2"],
      "plugins": ["istanbul"]
    }
  }
}
```

`以下可以不用安装`

## 安装 font-awesome
`cnpm i font-awesome --save-dev`

## 引入font-awesome
在**main.js**里面添加以下代码
```javascript
import 'font-awesome/css/font-awesome.css'
```

#### 使用代码：
```html
<i class="fa fa-fw fa-weibo"></i>
```

## 安装vux
`cnpm i vux --save-dev`
`cnpm i vux-loader`

## 引入vux
在**webpack.base.conf.js**里面添加/修改以下代码
```javascript
const vuxLoader = require('vux-loader')
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
在**index.html**里面添加以下代码
```html
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=0">
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


