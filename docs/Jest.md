### 安装Jest

`npm install --save-dev jest`

### 创建文件︰

*sum.js* 
```js
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

*sum.test.js*
```js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```
### 配置

*package.json*
```json
{
  "scripts": {
    "test": "jest"
  }
}
```

### 运行
 `yarn test`

### 结果 
```shell
PASS  ./sum.test.js
✓ adds 1 + 2 to equal 3 (5ms)
```