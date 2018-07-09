## 步骤

*创建文件夹lxl-cli-demo*

`cd lxl-cli-demo`

`npm init -y`

*package.json*
```json
{
  "name": "lxl-cli-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "bin":{
    "lxl-cli": "./bin/index.js"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

*创建./bin/index.js*
```js
`#!/usr/bin/env node`
console.log('hello cli')
```

`npm install -g`

## 使用

打开cmd，输入`lxl-cli`

## 后续
[用Node.js开发一个Command Line Interface (CLI)](https://zhuanlan.zhihu.com/p/38730825)