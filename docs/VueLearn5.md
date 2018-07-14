# Webpack

* webpack 将带有依赖项的各个模块打包处理后，变成了独立的浏览器能够识别的文件
* webpack 合并以及解析带有依赖项的模块

## 概述
* webpack 是一个现代 JavaScript 应用程序的模块打包器(特点 module、 bundler) 
* webpack 是一个模块化方案（预编译） 
* webpack获取具有依赖关系的模块，并生成表示这些模块的静态资源

四个核心概念：入口(entry)、输出(output)、加载器loader、插件(plugins)

### webpack起源

webpack解决了现存模块打包器的两个痛点：

* Code Spliting - 代码分离 按需加载
* 静态资源的模块化处理方案

### webpack与模块

[前端模块系统的演进](https://zhaoda.net/webpack-handbook/module-system.html)

在webpack看来：所有的静态资源都是模块

webpack 模块能够识别以下等形式的模块之间的依赖：

JS的模块化规范：

* ES2015 `import export`
* CommonJS `require()` `module.exports`
* AMD `define 和 require`

非JS等静态资源：

* css/sass/less 文件中的 `@import`
* 图片链接，比如：样式 `url(...)` 或 HTML `<img src=...>`
* 字体等

### webpack文档和资源

[webpack中文网](https://webpack.docschina.org/)

[入门Webpack，看这篇就够了](https://www.jianshu.com/p/42e11515c10f#)

#### 安装webpack

全局安装：`npm i -g webpack`

项目安装：`npm i -D webpack`

#### webpack的基本使用

安装：`npm i -D webpack`

webpack的两种使用方式：
* 命令行 
* 配置文件（webpack.config.js）

命令行方式演示 - 案例：隔行变色

1. 使用npm init -y 初始package.json，使用npm来管理项目中的包
2. 新建index.html和index.js，实现隔行变色功能
3. 运行`webpack src/js/index.js dist/bundle.js`进行打包构建，语法是：`webpack 入口文件 输出文件`

`npm i -D jquery`

*src/js/index.js*
```js
// 1 导入 jQuery
import $ from 'jquery'
// 2 获取页面中的li元素
const $lis = $('#ulList').find('li')
// 3 隔行变色
// jQuery中的 filter() 方法用来过滤jquery对象
$lis.filter(':odd').css('background-color', '#def')
$lis.filter(':even').css('background-color', 'skyblue')
```

命令行运行 `webpack src/js/index.js dist/bundle.js`

  运行流程：
  * webpack 根据入口找到入口文件
  * 分析js中的模块化语法 
  * 将所有关联文件,打包合并输出到出口

### webpack-dev-server 配置

#### *package.json* 配置方式

* 安装：`npm i -D webpack-dev-server`
* 作用：配合webpack，创建开发环境（启动服务器、监视文件变化、自动编译、刷新浏览器等），提高开发效率
* 注意：无法直接在终端中执行 webpack-dev-server，需要通过 package.json 的 scripts 实现
* 使用方式：`npm run dev`

```js
// 参数解释  注意参数是无序的 有值的参数空格隔开
// --open 自动打开浏览器
// --contentBase ./  指定浏览器 默认打开的页面路径中的 index.html 文件
// --open 自动打开浏览器
// --port 8080 端口号
// --hot 热更新，只加载修改的文件(按需加载修改的内容)，而非全部加载
"scripts": {
  "dev": "webpack-dev-server --open --contentBase ./ --port 8080 --hot"
}
```

#### *webpack.config.js* 配置方式(推荐)

```js
var path = require('path')
module.exports = {
  // 入口文件
  entry: path.join(__dirname, 'src/js/index.js'),

  // 输出文件
  output: {
    path: path.join(__dirname, 'dist'),   // 输出文件的路径
    filename: 'bundle.js'                 // 输出文件的名称
  }
}

const webpack = require('webpack')

devServer: {
  // 服务器的根目录 Tell the server where to serve content from
  // https://webpack.js.org/configuration/dev-server/#devserver-contentbase
  contentBase: path.join(__dirname, './'),
  // 自动打开浏览器
  open: true,
  // 端口号
  port: 8888,

  // --------------- 1 热更新 -----------------
  hot: true
},

plugins: [
  // ---------------- 2 启用热更新插件 ----------------
  new webpack.HotModuleReplacementPlugin()
]
```

#### html-webpack-plugin 插件

* 安装：`npm i -D html-webpack-plugin`
* 作用：根据模板，自动生成html页面
* 优势：页面存储在内存中，自动引入bundle.js、css等文件

*webpack.config.js*
```js
const htmlWebpackPlugin = require('html-webpack-plugin')
plugins: [
  new htmlWebpackPlugin({
    // 模板页面路径
    template: path.join(__dirname, './index.html'),
    // 在内存中生成页面路径，默认值为：index.html
    filename: 'index.html'
  })
]
```

### [Loaders加载器](https://webpack.docschina.org/concepts/loaders/)

* webpack只能处理JavaScript资源
* webpack通过loaders处理非JavaScript静态资源

1. CSS打包
* 安装：`npm i -D style-loader css-loader`
* 注意：use中模块的顺序不能颠倒，加载顺序：从右向左加载

*index.js*
```js
/* 导入 css 文件*/
import './css/app.css'

/* webpack.config.js 配置各种资源文件的loader加载器*/
module: {
  // 配置匹配规则
  rules: [
    // test 用来配置匹配文件规则（正则）
    // use  是一个数组，按照从后往前的顺序执行加载
    {test: /\.css$/, use: ['style-loader', 'css-loader']},
  ]
}
```
2. 使用webpack打包sass文件
* 安装：npm i -D sass-loader
* 注意：sass-loader 依赖于 node-sass 模块

*webpack.config.js*
```js
// 参考：https://webpack.js.org/loaders/sass-loader/#examples
// "style-loader"  ：creates style nodes from JS strings 创建style标签
// "css-loader"    ：translates CSS into CommonJS 将css转化为CommonJS代码
// "less-loader"   ：compiles Less to CSS 将Less编译为css
module:{
  rules:[
    {
      test: /\.less$/,
      loader: "style-loader!css-loader!less-loader"
    }
  ]
}
```

3. 图片和字体打包
* 安装：`npm i -D url-loader file-loader`
* `file-loader`：加载并重命名文件（图片、字体 等）
* `url-loader`：将图片或字体转化为base64编码格式的字符串，嵌入到样式文件中

*webpack.config.js*
```js
module: {
  rules:[
    // 打包 图片文件
    { test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader' },

    // 打包 字体文件
    { test: /\.(woff|woff2|eot|ttf|otf)$/, use: 'file-loader' }
  ]
}
```

#### 图片打包细节

* limit参数的作用：（单位为：字节(byte)）

  * 当图片文件大小（字节）小于指定的limit时，图片被转化为base64编码格式
  * 当图片文件大小（字节）大于等于指定的limit时，图片被重命名以url路径形式加载（此时，需要file-loader来加载图片）
* 图片文件重命名，保证相同文件不会被加载多次。例如：一张图片（a.jpg）拷贝一个副本（b.jpg），同时引入这两张图片，重命名后只会加载一次，因为这两张图片就是同一张
* 文件重命名以后，会通过MD5加密的方式，来计算这个文件的名称

*webpack.config.js*
```js
module: {
  rules: [
    // {test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader?limit=100'},
    {
      test: /\.(jpg|png|gif|jpeg)$/,
      use: [
        {
          loader: 'url-loader',
          options: {
            limit: 8192
          }
        }
      ]
    }
  ]
}
```

#### 字体文件打包说明

处理方式与图片相同，可以使用：`file-loader`或`url-loader`

# Babel

* 安装：npm i -D babel-core babel-loader
* 安装：npm i -D babel-preset-env

## 基本使用

*webpack.config.js*
```js
module: {
  rules: [
    // exclude 排除，不需要编译的目录，提高编译速度
    {test: /\.js$/, use: 'babel-loader', exclude: /node_modules/}
  ]
}
```

在项目根目录中新建.babelrc配置文件

*babelrc文件*
```js
// 将来babel-loader运行的时候，会检查这个配置文件，并读取相关的语法和插件配置
{
  "presets": ["env"]
}
```

## babel的说明

### babel的作用：

1. 语法转换：将新的ES语法转化为浏览器能识别的语法（babel-preset-*）
2. polyfill浏览器兼容：让低版本浏览器兼容最新版ES的API

### babel-preset-*

> Babel通过语法转换器，能够支持最新版本的JavaScript语法 

> `babel-preset-*` 用来指定我们书写的是什么版本的JS代码

### `babel-polyfill` 和 `transform-runtime`

* 作用：实现浏览器对不支持API的兼容（兼容旧环境、填补）
  > 在低版本浏览器中使用高级的ES6或ES7的方法或函数，比如：'abc'.padStart(10)

1. polyfill `npm i -S babel-polyfill`
2. transform-runtime `npm i -D babel-plugin-transform-runtime 和 npm i -S babel-runtime`

* 注意：babel-runtime包中的代码会被打包到你的代码中（-S）

* 区别：

  * polyfill 所有兼容性问题，都可以通过polyfill解决（包括：实例方法）、污染全局环境
  * runtime  除了实例方法以外，其他兼容新问题都能解决、不污染全局环境
  * polyfill：如果想要支持全局对象（比如：`Promise`）、静态方法（比如：`Object.assign`）或者实例方法（比如：`String.prototype.padStart`）等，那么就需要使用`babel-polyfill`
  * babel-runtime ：提供了兼容旧环境的函数，使用的时候，需要我们自己手动引入

  比如： const Promise = require('babel-runtime/core-js/promise')

  存在的问题：
  1. 手动引入太繁琐
  2. 多个文件引入同一个helper（定义），造成代码重复，增加代码体积

### babel-plugin-transform-runtime：
  1. 自动引入helper（比如，上面引入的 Promise）
  2. babel-runtime提供helper定义，引入这个helper即可使用，避免重复
  3. 依赖于 babel-runtime 插件

### transform-runtime插件的使用：
直接在 `.bablerc` 文件中，添加一个 plugins 的配置项即可！！！

```js
"plugins": [
  "transform-runtime"
]
```

### babel-polyfill 的使用步骤：

1. *main.js*
```js
// 第一行引入
require("babel-polyfill")

var s = 'abc'.padStart(4)
console.log(s)
```

2. *webpack.config.js* 配置
```js
module.exports = {
  entry: ['babel-polyfill', './js/main.js']
}
```

## 总结:

* `babel-core` babel核心包

* `babel-loader` 用来解析js文件

* `babel-preset-*` 新ES语法的解析和转换

* `transform-runtime / babel-polyfill` 兼容旧浏览器，到达支持新API目的

