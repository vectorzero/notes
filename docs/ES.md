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

## Array.prototype.includes
用于快速查找数组中是否包含某个元素。(包括NaN，所以和indexOf不一样)。
```js
const arr = [1,2,3,NaN];
console.log(arr.includes(3)) // true
console.log(arr.indexOf(3) > 0) // true
console.log(arr.includes(NaN)) // true
console.log(arr.indexOf(NaN) > 0) // false
```

## 指数函数的中缀形式
可以使用 `**` 来替代 `Math.pow`
```js
Math.pow(7,2) // 49
7**2 // 49
```

## Object.values()
`Object.values()` 函数和 `Object.keys()` 很相似，它返回一个对象中自己属性的所有值(通过原型链继承的不算)。
```js
const objs = [{A: 3},{B: 4},{C: 5}];
const vals = Object.keys(objs).map(key => objs[key]); // [3,4,5]

const values = Object.values(objs); // [3,4,5]
```

## Object.entries()
`Object.entries()` 和 `Object.keys` 相关，不过 `entries()` 函数会将key和value以数组的形式都返回。
```js
Object.keys(objs).forEach(key => {
    console.log(`key: ${key},value:${objs[key]}`)
})
for(let [key,value] of Object.entries(objs)) {
    console.log(`key: ${key},value:${value}`)
}
```
