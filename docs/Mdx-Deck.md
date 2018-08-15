# [mdx-deck](https://github.com/jxnblk/mdx-deck)

*支持`markdown`,`react`写法*

### 创建 *index.mdx*
```html
<!-- index.mdx -->
# This is the title of my deck
---
# About Me
---
# The end
---
```

### 初始化
`npm init`

### 安装依赖
`npm i -D mdx-deck`

`npm i -D babel-core`

### 修改 *package.json*
```js
"scripts": {
  "start": "mdx-deck index.mdx",
  "build": "mdx-deck build index.mdx",
  "pdf": "mdx-deck pdf index.mdx"
}
```

### 启动项目
`npm start`