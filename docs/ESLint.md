### 安装 *ESLint*

`npm i -g eslint`

### 创建配置文件

配置规则可查看[官方文档](http://eslint.cn/docs/user-guide/configuring)

在根目录下创建 **.eslintrc.js** ，简单配置一下：

```js
module.exports = {
    extends: 'eslint:recommended',
    env: {
    browser: true,
    node: true,
    commonjs: true,
    es6: true,
    mocha: true
    },
    root: true,
    rules: {
    'no-console': 'off'
    }
}
```

### 验证代码质量与规范

创建一个 **app.js** 文件，输入一些内容，然后通过以下命令执行验证：

`eslint app.js `

 如果你的代码不符合要求，那么控制台并会提示你哪些地方存在问题。