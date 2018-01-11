# ES6

## 模板字符串
```js
let a = ~~(Math.random())*10;
console.log(`hello,${a}`);
```

## 扩展运算符
```js
const a = [1,2,3];
const b = [4,...a,6];
console.log(b); //[4,1,2,3,6]

const {c,d,...z} = {c:1,d:2,e:4,f:6};
console.log(c) //1
console.log(d) //2
console.log(z) //{e:4,f:6}
```

## Array.find 简写
```js
const arr = [
    {name:'aa',value:'1',type:'a'},
    {name:'bb',value:'2',type:'b'},
    {name:'cc',value:'3',type:'c'},
    {name:'cc',value:'4',type:'d'}    
]
const obj = arr.find(item => item.name === 'aa' && item.value === '1');
console.log(obj) // {name:'aa',value:'1',type:'a'}
```

## 跨模块常量
```javascript
//a.js
export const A = 1;
export const B = 2;
```
```javascript
//b.js
import ^ as hello from ‘./a.js’;
console.log(hello.A);//1
console.log(hello.B);//2
```
## 解构赋值

如果解构不成功。变量的值就等于`undefined`

解构赋值允许指定默认值
```javascript
var [ foo = true ] = [];
foo // true
[ x,y = ‘b’ ] = [‘a’] //x=’a’,y=’b’
[ x,y = ‘b’] = [‘a’, undefined] //x=’a’ ,y=’b’
```
注意：ES6内部使用严格相等运算符(===)判断一个位置是否有值。所以，如果一个数组成员不严格等于`undefined`，默认值是不会生效的。
```javascript
var [x = 1] = [undefined];
x // 1
var [x = 1] = [null];
x //null
```

如果将一个已经声明的变量用于解构赋值，必须非常小心
```javascript
// 错误的写法
var x;
{x} = {x:1};
// SyntaxError syntax error 
```
上面的代码写法会报错，因为js引擎会将{x}理解成一个代码块，从而发生语法错误，只有不将大括号写在行首，避免js将其解释为代码块，才能解决这个问题。
```javascript
// 正确的写法
var x ;
({x}={x:1});
```