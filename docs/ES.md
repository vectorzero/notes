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

## Array.find
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

## 字符串追加
提供了两个字符串追加的方法`String.prototype.padStart`和`String.prototype.padEnd`，方便我们将一个新的字符串追加到某个字符串的头尾。

`'someString'.padStart(numberOfCharcters [,stringForPadding]); `

```js
'5'.padStart(10) // '          5'
'5'.padStart(10, '*') // '*********5'
'5'.padStart(10, '*&') // '*&*&*&*&*5'
'5'.padEnd(10) // '5         '
'5'.padEnd(10, '*') //'5*************'
'5'.padEnd(10, '*&') // '5*&*&*&*&*'
```
```js
//输出格式化
const cars = {
  '🚙BMW': '10',
  '🚘Tesla': '5',
  '🚖Lamborghini': '0'
}
Object.entries(cars).map(([name, count]) => {
  //padEnd appends ' -' until the name becomes 20 characters
  //padStart prepends '0' until the count becomes 3 characters.
  console.log(`${name.padEnd(20, ' -')} Count: ${count.padStart(3, '0')}`)
});
//Prints..
// 🚙BMW - - - - - - -  Count: 010
// 🚘Tesla - - - - - -  Count: 005
// 🚖Lamborghini - - -  Count: 000

```

## Object.getOwnPropertyDescriptors

该函数返回一个对象所有的属性，甚至包括`get/set`函数。

*ES2017*加入这个函数的主要动机在于方便将一个对象深度拷贝给另一个对象，同时可以将`getter/setter`拷贝。

和`Object.assign`不同，`Object.assign`将一个对象除了`getter/setter`以外的都深度拷贝了。

```js
let Hello = {
    name: 'xiaoming',
    age: '25',
    set nowAge(x) {
        this.a = x;
    },
    get nowAge() {
        return this.a
    }
}
console.log(Object.getOwnPropertyDescriptors(Hello,'nowAge'));
/*
{ name:
   { value: 'xiaoming',
     writable: true,
     enumerable: true,
     configurable: true },
  age:
   { value: '25',
     writable: true,
     enumerable: true,
     configurable: true },
  nowAge:
   { get: [Function: get nowAge],
     set: [Function: set nowAge],
     enumerable: true,
     configurable: true } }
*/
const copyHelloI = Object.assign({},Hello);
console.log(Object.getOwnPropertyDescriptors(copyHelloI,'nowAge'));
/*
{ name:
   { value: 'xiaoming',
     writable: true,
     enumerable: true,
     configurable: true },
  age:
   { value: '25',
     writable: true,
     enumerable: true,
     configurable: true },
  nowAge:
   { value: undefined,
     writable: true,
     enumerable: true,
     configurable: true } }
*/
const copyHelloII = Object.defineProperties({},Object.getOwnPropertyDescriptors(Hello));
console.log(Object.getOwnPropertyDescriptors(copyHelloII,'nowAge'));
/*
{ name:
   { value: 'xiaoming',
     writable: true,
     enumerable: true,
     configurable: true },
  age:
   { value: '25',
     writable: true,
     enumerable: true,
     configurable: true },
  nowAge:
   { get: [Function: get nowAge],
     set: [Function: set nowAge],
     enumerable: true,
     configurable: true } }
*/
```

## Async/Await

`async`关键字告诉JavaScript编译器对于标定的函数要区别对待。当编译器遇到`await`函数的时候会暂停。它会等到`await`标定的函数返回的`promise`。该`promise`要么得到结果、要么`reject`。

在下面的例子中，`getAmount`函数调用`getUser`和`getBankBalance`两个异步函数。我们可以用`promise`来实现它，不过用`async await`更加简洁。

```js
//ES2015 Promise
function getAmountI(userId) {
  getUser(userId)
    .then(getBankBlance)
    .then(amount => {
      console.log(amount)
    })  
}

//ES2017 Async/Await
async function getAmountII(userId) {
  const user = await getUser(userId);
  const amount = await getBankBlance(user);
  console.log(amount);
}

function getUser(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('join')
    },1000)
  })
}

getAmountI('1'); //$1000
getAmountII('1'); //$1000

function getBankBlance(user) {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      if(user === 'join') {
        resolve('$1000')
      }else {
        reject('unknown user')
      }
    },1000)
  })
}
```

### async函数返回Promise

如果你想获取一个async函数的结果，你需要使用Promise的then语法。

在下面的例子中，我们想用console.log来打印doubleAndAdd的结果，可以使用then语法，将console.log函数作为参数传入。
```js
async function hello(a,b) {
a = await hey(a);
b = await hey(b);
return a + b;
}

hello(1,2).then(console.log) //6

function hey(param) {
return new Promise(resolve => {
    setTimeout(() => {
    resolve(param * 2)
    },1000)
})
}
```

### 并行处理

在上面的例子中，我们显示地调用了await两次，因为每次都等待了1秒钟，因此总计两秒钟。现在，我们可以使用Promise.all函数来让他们并行处理。

```js
async function hello(a,b) {
    [a,b] = await Promise.all([hey(a),hey(b)]);
    return a + b;
}
```