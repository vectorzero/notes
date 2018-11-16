## 短路表达式
作为"&&"和"||"操作符的操作数表达式，这些表达式在进行求值时，只要最终的结果已经可以确定是真或假，求值过程便告终止，这称之为短路求值。这是这两个操作符的一个重要属性。

```javascript
    // ||短路表达式
    var foo = a || b;
    // 相当于
    if(a){
        foo = a;
    }else{
        foo = b;
    }
    
    // &&短路表达式
    var bar = a && b;
    // 相当于
    if(a){
        bar = b;
    }else{
        bar = a;
    }
```

1、在 Javascript 的逻辑运算中，0、""、null、false、undefined、NaN 都会判定为 false ，而其他都为 true ；

2、因为 Javascript 的内置弱类型域 (weak-typing domain)，所以对严格的输入验证这一点不太在意，即便使用 && 或者 || 运算符的运算数不是布尔值，仍然可以将它看作布尔运算。虽然如此，还是建议如下：

```javascript
if(foo){ ... }     //不够严谨
if(!!foo){ ... }   //更为严谨，!!可将其他类型的值转换为boolean类型
```

## 十进制指数
```javascript
console.log(1e2) //100
```

## 节流*throttle* & 防抖*debounce*
*throttle* 可以想象成阀门一样定时打开来调节流量。 

*debounce*可以想象成把很多事件压缩组合成了一个事件。

有个生动的类比，假设你在乘电梯，没次进一个人需等待10秒钟，不考虑电梯容量限制，那么两种不同的策略是：

*debounce*: 你在进入电梯后发现这时不远处走来了了一个人，等10秒钟，这个人进电梯后不远处又有个妹纸姗姗来迟，怎么办，再等10秒，于是妹纸上电梯时又来了一对好基友...，作为感动中国好码农，你要每进一个人就等10秒，直到没有人进来，10秒超时，电梯开动。

*throttle*: 电梯很忙，每次就只等10秒，不管是来了妹纸还是好基友，电梯每隔10秒准时送一波人。

因此，简单来说，*debounce*适合只执行一次的情况，例如 搜索框中的自动完成。在停止输入后提交一次ajax请求;

而*throttle*适合指定每隔一定时间间隔内执行不超过一次的情况，例如拖动滚动条，移动鼠标的事件处理等。

### 防抖
```html
<input />
<div id="res"></div>
```
```javascript
const $input = document.querySeletor('input');
const $res = document.getElementById('res');
function debounce(doSomething,delay) {
    let timeout;
    return function() {
        const _this = this;
        const _arguments = arguments;
        clearTimeout(timeout);
        timeout = setTiemout(() => {
            doSomething.apply(_this,_arguments);
        },delay)
    }
}
let validate = debounce(function(e){
    $res.innerHTML = e.target.value;
},300)
$input.addEventListener('input',validate);

```

### 节流
```html
<div id="div" style="width: 100px; height: 100px; background: #ddaee1"></div>
<p id="throRes"></p>
<p id="throResII"></p>
```
```javascript
const $div = document.getElementById('div');
const $throRes = document.getElementById('throRes');
const $throResII = document.getElementById('throResII');
function throttle(doThrottle,threshhold = 160) {
    let timeout;
    let start = new Date;
    return function() {
        const _this = this;
        const _arguments = arguments;
        let current = new Date() - 0;

        //总是干掉事件回调
        clearTimeout(timeout);
        if(current - start >= threshhold) {

            //只执行一部分方法，这些方法是在某个时间段内执行一次
            doThrottle.apply(_this,_arguments);
            start = current;
        }else {

            //让方法在脱离事件后也能执行一次
            timeout = setTimeout(() => {
                doThrottle.apply(_this, _arguments)
            },threshhold)
        }
    }
}
let mouseMove = throttle(function(e) {
    $throRes.innerHTML = `throttle__${e.pageX},${e.pageY}`;
})
let mouseMoveII = function(e) {
    $throResII.innerHTML = `normal__${e.pageX},${e.pageY}`;
}
$div.addEventListener('mousemove',mouseMove);
$div.addEventListener('mousemove',mouseMoveII);
```

那什么时候该用 debounce 什么时候该用 throttle 呢？

* input 中输入文字自动发送 ajax 请求进行自动补全： debounce
* resize window 重新计算样式或布局：debounce
* mouseleave 时隐藏二级菜单：debounce，并合理使用 cancel 方法
* scroll 时更新样式，如随动效果：throttle

最重要的还是理解两者对调用时间及次数上的处理，根据业务逻辑选择最合适的优化方案！

## JSON.parse() & JSON.stringify()

### JSON.parse()

可以接受第二个参数，它可以在返回之前转换对象值。

```javascript
const obj = {
    name: 'xiaoming',
    age: 18,
    sex: '男'
}
const stringObj = JSON.stringify(obj);
// 将返回对象的属性值大写：
const resultObj = JSON.parse(stringObj,(key,value) => {
    if(typeof value === 'string') {
        return value.toUpperCase()
    }
    return value
});
console.log(resultObj) // { name: 'XIAOMING', age: 18, sex: '男' }
```

### JSON.stringify()

可以带两个额外的参数，第一个是替换函数，第二个间隔字符串，用作隔开返回字符串。

参数：

1. value ： 将要转为JSON字符串的javascript对象。
2. replacer ：该参数可以是多种类型,如果是一个函数,则它可以改变一个javascript对象在字符串化过程中的行为, 如果是一个包含 String 和 Number 对象的数组,则它将作为一个白名单.只有那些键存在域该白名单中的键值对才会被包含进最终生成的JSON字符串中.如果该参数值为null或者被省略,则所有的键值对都会被包含进最终生成的JSON字符串中。
3. space ：该参数可以是一个 String 或 Number 对象,作用是为了在输出的JSON字符串中插入空白符来增强可读性. 如果是Number对象, 则表示用多少个空格来作为空白符; 最大可为10,大于10的数值也取10.最小可为1,小于1的数值无效,则不会显示空白符. 如果是个 String对象, 则该字符串本身会作为空白符,字符串最长可为10个字符.超过的话会截取前十个字符. 如果该参数被省略 (或者为null), 则不会显示空白符

```javascript
const obj = {
    name: 'xiaoming',
    age: 18,
    sex: '男'
}

// 替换函数可以用来过滤值，因为任何返回 `undefined` 的值将不在返回的字符串中：
function deleteAge(key,value) {
    if(key === 'sex') {
        return undefined
    }
    return value
}
const resultObj = JSON.stringify(obj,deleteAge,null);
console.log(resultObj) // {"name":"xiaoming","age":18}

// 传入一个间隔参数
const resultObjII = JSON.stringify(obj,null,'...');
console.log(resultObjII)
// {
// ..."name": "xiaoming",
// ..."age": 18,
// ..."sex": "男"
// }
const resultObjIII = JSON.stringify(obj,null,4);
console.log(resultObjIII)
// {
//     "name": "xiaoming",
//     "age": 18,
//     "sex": "男"
// }
```

### toJSON

利用toJSON方法,我们可以修改对象转换成JSON的默认行为。

如果一个被序列化的对象拥有 toJSON 方法，那么该 toJSON 方法就会覆盖该对象默认的序列化行为：不是那个对象被序列化，而是调用 toJSON 方法后的返回值会被序列化

```javascript
const obj = {
    name: 'xiaoming',
    toJSON: function() {
        return 'hello'
    }
}
console.log(JSON.stringify(obj)) // "hello"
console.log(JSON.stringify({x:obj})) // {"x":"hello"}
```

### 用 JSON.stringify 来格式化对象

在实际使用中,我们可能会格式化一些复杂的对象，这些对象往往对象内嵌套对象。直接看起来并不那么直观,结合上面的的 replacer 和 space 参数,我们可以这样格式化复杂对象：

``` javascript
var censor = function(key,value){
    if(typeof(value) == 'function'){
         return Function.prototype.toString.call(value)
    }
    return value;
}
var foo = {bar:"1",baz:3,o:{name:'xiaoli',age:21,info:{sex:'男',getSex:function(){return 'sex';}}}};
console.log(JSON.stringify(foo,censor,4))
```
``` JSON
{
    "bar": "1",
    "baz": 3,
    "o": {
        "name": "xiaoli",
        "age": 21,
        "info": {
            "sex": "男",
            "getSex": "function (){return 'sex';}"
        }
    }
}
```
