## 步骤

### 创建文件夹

*lxl-cli-demo*

### 执行命令

`cd lxl-cli-demo`

`npm init -y`

### 创建文件

*./bin/index.js*
```js
#!/usr/bin/env node
console.log('hello cli')
```

### 修改文件

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

### 全局安装

`npm install -g`

## 使用

打开cmd，输入`lxl-cli`

## 后续
[用Node.js开发一个Command Line Interface (CLI)](https://zhuanlan.zhihu.com/p/38730825)