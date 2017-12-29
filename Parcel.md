# Parcel & React || Vue || TypeScript

### 安装 Parcel

*node 7.6 以上*

`npm install -g parcel-bundler`
`npm init -y`

*index.html*

```html
<html>
<body>
  <script src="./index.js"></script>
</body>
</html>
```
默认地址端口为 [localhost:1234](http://localhost:1234)
修改端口 `parcel index.html -p 3304`

### 集成技术栈

新建 index-react.html、index-vue.html、index-ts.html 入口文件

*package.json*
```javascript
"scripts": {
  "react": "parcel index-react.html",
  "vue": "parcel index-vue.html",
  "ts": "parcel index-ts.html"
}
```

#### React

新建 *.babelrc*
```
{
  "presets": ["env","react"]
}
```

*index-react.html*
```html
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

*src/react/index.js*
```javascript
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

*index-vue.html*
```html
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

*src/vue/index.js*
```javascript
import Vue from 'vue';
import App from './app.vue';
new Vue({
    el: '#vue-app',
    render: h => h(App)
})
```

* src/vue/app.vue*
```
<template>
    <div>
        <h1>Hello Vue</h1>
    </div>
</template>
```

`npm run vue`

#### TypeScript
`npm i -S typescript`

*index-ts.html*
```html
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

*src/typescript/index.ts*
```javascript
interface Name {
    value: string;
}
function showName(name: Name){
    document.getElementById('ts-app').innerHTML = 'Hello ' + name.value;
}
showName({value: 'TypeScript'});
```

`npm run ts`
