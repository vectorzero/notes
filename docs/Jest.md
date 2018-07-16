`npm install --save-dev jest`

让我们从写一个两个数相加的示例函数开始。首先，创建一个 *sum.js* 文件︰
```js
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```
然后，创建一个名为 *sum.test.js* 的文件。这将包含我们的实际测试︰
```js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```
将下面的配置部分添加到你的 *package.json* 里面：
```json
{
  "scripts": {
    "test": "jest"
  }
}
```
最后，运行 `yarn test`， Jest将打印以下消息：
```shell
PASS  ./sum.test.js
✓ adds 1 + 2 to equal 3 (5ms)
```
你刚刚成功地写了第一个Jest测试 ！