# Parcel & React || Vue || TypeScript

### 快速使用

*node 7.6 以上*

`npm install -g parcel-bundler`

`npm init -y`

```html
<!-- 新建 index.html -->
<html>
<body>
  <script src="./index.js"></script>
</body>
</html>
```
默认地址端口为 [localhost:1234](http://localhost:1234)

修改端口示例 `parcel index.html -p 3304`

### 集成技术栈

```javascript
//package.json
"scripts": {
  "react": "parcel index-react.html",
  "vue": "parcel index-vue.html",
  "ts": "parcel index-ts.html"
}
```

#### React

`npm i -S parcel-bundler react react-dom babel-preset-env babel-preset-react`

```javascript
//新建 .babelrc
{
  "presets": ["env","react"]
}
```

```html
<!-- 新建 index-react.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Parcel React</title>
        <meta charset="UTF-8">
    </head>
    <body>
        <div id="react-app"></div>
        <script src="src/react/index.js"></script>
    </body>
</html>
```

```javascript
//新建 src/react/index.js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
class Hello extends Component {
    render() {
        return <h1>Hello React</h1>;
    }
}
ReactDOM.render(<Hello />, document.getElementById('react-app'));
```

`npm run react`

#### Vue

`npm i -S vue parcel-plugin-vue`

```html
<!-- 新建 index-vue.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Parcel Vue</title>
        <meta charset="UTF-8">
    </head>
    <body>
        <div id="vue-app"></div>
        <script src="src/vue/index.js"></script>
    </body>
</html>
```

```javascript
//新建 src/vue/index.js
import Vue from 'vue';
import App from './app.vue';
new Vue({
    el: '#vue-app',
    render: h => h(App)
})
```

```html
<!-- 新建 src/vue/app.vue -->
<template>
    <div>
        <h1>Hello Vue</h1>
    </div>
</template>
```

`npm run vue`

#### TypeScript
`npm i -S typescript`

```html
<!-- 新建 index-ts.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Parcel TypeScript</title>
        <meta charset="UTF-8">
    </head>
    <body>
        <h1 id="ts-app"></h1>
        <script src="src/typescript/index.ts"></script>
    </body>
</html>
```

```typescript
//新建 src/typescript/index.ts
interface Name {
    value: string;
}
function showName(name: Name){
    document.getElementById('ts-app').innerHTML = 'Hello ' + name.value;
}
showName({value: 'TypeScript'});
```

`npm run ts`

### 生产环境

设置环境变量： `parcel build index.html NODE_ENV=production`

设置输出目录： `parcel build index.html -d build/output`

设置要提供服务的公共 URL：`parcel build index.html --public-url ./`

禁用压缩： `parcel build index.html --no-minify`

禁用文件系统缓存： `parcel build index.html --no-cache`