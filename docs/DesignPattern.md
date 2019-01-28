## [设计模式](https://github.com/dongyuanxin/design-patterns)

1. <a herf="#单例模式">单例模式</a>
2. <a herf="#策略模式">策略模式</a>
3. <a herf="#代理模式">代理模式</a>
4. <a herf="#迭代器模式">迭代器模式</a>
5. <a herf="#订阅发布模式">订阅发布模式</a>
6. <a herf="#组合模式">组合模式</a>
7. <a herf="#享元模式">享元模式</a>
8. <a herf="#责任链模式">责任链模式</a>
9. <a herf="#组合模式">组合模式</a>
10. <a herf="#享元模式">享元模式</a>
11. <a herf="#责任链模式">责任链模式</a>
12. <a herf="#装饰者模式">装饰者模式</a>

### <a name="单例模式">单例模式</a>

#### 描述

> 单例模式定义：保证一个类仅有一个实例，并提供访问此实例的全局访问点。

#### 单例模式用途

如果一个类负责连接数据库的线程池、日志记录逻辑等等，此时需要单例模式来保证对象不被重复创建，以达到降低开销的目的。

#### 代码实现

```js
const Singleton = function() {};

Singleton.getInstance = (function() {
  // 由于es6没有静态类型,故闭包: 函数外部无法访问 instance
  let instance = null;
  return function() {
    // 检查是否存在实例
    if (!instance) {
      instance = new Singleton();
    }
    return instance;
  };
})();

let s1 = Singleton.getInstance();
let s2 = Singleton.getInstance();

console.log(s1 === s2);
```

### <a name="策略模式">策略模式</a>

#### 描述

> 策略模式定义：就是能够把一系列“可互换的”算法封装起来，并根据用户需求来选择其中一种。

策略模式实现的核心就是：将算法的使用和算法的实现分离。算法的实现交给策略类。算法的使用交给环境类，环境类会根据不同的情况选择合适的算法。

#### 策略模式优缺点

在使用策略模式的时候，需要了解所有的“策略”（strategy）之间的异同点，才能选择合适的“策略”进行调用。

#### 代码实现

```js
// 策略类
const strategies = {
  A() {
    console.log("This is stragegy A");
  },
  B() {
    console.log("This is stragegy B");
  }
};

// 环境类
const context = name => {
  return strategies[name]();
};

// 调用策略A
context("A");
// 调用策略B
context("B");
```

### <a name="代理模式">代理模式</a>

#### 描述

> 代理模式的定义：为一个对象提供一种代理以方便对它的访问。

代理模式可以解决避免对一些对象的直接访问，以此为基础，常见的有保护代理和虚拟代理。

保护代理可以在代理中直接拒绝对对象的访问；

虚拟代理可以延迟访问到真正需要的时候，以节省程序开销。

#### 代理模式优缺点

代理模式有高度解耦、对象保护、易修改等优点。

同样地，因为是通过**代理**访问对象，因此开销会更大，时间也会更慢。

#### 代码实现

```js
const myImg = {
  setSrc(imgNode, src) {
    imgNode.src = src;
  }
};

// 利用代理模式实现图片懒加载
const proxyImg = {
  setSrc(imgNode, src) {
    myImg.setSrc(imgNode, "./image.png"); // NO1. 加载占位图片并且将图片放入<img>元素

    let img = new Image();
    img.onload = () => {
      myImg.setSrc(imgNode, src); // NO3. 完成加载后, 更新 <img> 元素中的图片
    };
    img.src = src; // NO2. 加载真正需要的图片
  }
};

let imgNode = document.createElement("img"),
  imgSrc =
    "https://upload-images.jianshu.io/upload_images/5486602-5cab95ba00b272bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp";

document.body.appendChild(imgNode);

proxyImg.setSrc(imgNode, imgSrc);
```

### <a name="迭代器模式">迭代器模式</a>

#### 描述

> 迭代器模式是指提供一种方法顺序访问一个集合对象的各个元素，使用者不需要了解集合对象的底层实现。

迭代器模式常见和常用的有：内部迭代器、外部迭代器、倒序迭代器等等。

#### 内部迭代器和外部迭代器

内部迭代器：封装的方法完全接手迭代过程，外部只需要一次调用。

外部迭代器：用户必须显式地请求迭代下一元素。熟悉C++的朋友，可以类比C++内置对象的迭代器的`end()`、`next()`等方法。

#### 代码实现

这里实现的是一个外部迭代器。需要实现边界判断函数、元素获取函数和更新索引函数。

```js
const Iterator = obj => {
  let current = 0;
  let next = () => current += 1;
  let end = () => current >= obj.length;
  let get = () => obj[current];

  return {
    next,
    end,
    get
  }
}

let myIter = Iterator([1, 2, 3]);
while(!myIter.end()) {
  console.log(myIter.get())
  myIter.next();
}
```

### <a name="订阅发布模式">订阅发布模式</a>

#### 描述

> 订阅-发布模式定义了对象之间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖它的对象都可以得到通知。

了解过事件机制或者函数式编程的朋友，应该会体会到**订阅-发布模式**所带来的**时间解耦**和**空间解耦**的优点。

借助函数式编程中闭包和回调的概念，可以很优雅地实现这种设计模式。

#### 订阅-发布模式 vs 观察者模式

订阅-发布模式和观察者模式概念相似，但在订阅-发布模式中，订阅者和发布者之间多了一层中间件：一个被抽象出来的信息调度中心。

但其实没有必要太深究 2 者区别，因为《Head First 设计模式》这本经典书都写了：发布+订阅=观察者模式。

**其核心思想是状态改变和发布通知**。在此基础上，根据语言特性，进行实现即可。

#### 代码实现

JS中一般用事件模型来代替传统的发布-订阅模式。任何一个对象的原型链被指向`Event`的时候，这个对象便可以绑定自定义事件和对应的回调函数。

```js
const Event = {
  clientList: {},

  // 绑定事件监听
  listen(key, fn) {
    if (!this.clientList[key]) {
      this.clientList[key] = [];
    }
    this.clientList[key].push(fn);
    return true;
  },

  // 触发对应事件
  trigger() {
    const key = Array.prototype.shift.apply(arguments),
      fns = this.clientList[key];

    if (!fns || fns.length === 0) {
      return false;
    }

    for (let fn of fns) {
      fn.apply(null, arguments);
    }

    return true;
  },

  // 移除相关事件
  remove(key, fn) {
    let fns = this.clientList[key];

    // 如果之前没有绑定事件
    // 或者没有指明要移除的事件
    // 直接返回
    if (!fns || !fn) {
      return false;
    }

    // 反向遍历移除置指定事件函数
    for (let l = fns.length - 1; l >= 0; l--) {
      let _fn = fns[l];
      if (_fn === fn) {
        fns.splice(l, 1);
      }
    }

    return true;
  }
};

// 为对象动态安装 发布-订阅 功能
const installEvent = obj => {
  for (let key in Event) {
    obj[key] = Event[key];
  }
};

let salesOffices = {};
installEvent(salesOffices);

// 绑定自定义事件和回调函数

salesOffices.listen(
  "event01",
  (fn1 = price => {
    console.log("Price is", price, "at event01");
  })
);

salesOffices.listen(
  "event02",
  (fn2 = price => {
    console.log("Price is", price, "at event02");
  })
);

salesOffices.trigger("event01", 1000);
salesOffices.trigger("event02", 2000);

salesOffices.remove("event01", fn1);

// 输出: false
// 说明删除成功
console.log(salesOffices.trigger("event01", 1000));
```

### <a name="命令模式">命令模式</a>

#### 描述

> 命令模式是一种数据驱动的设计模式，它属于行为型模式。

1. 请求以命令的形式包裹在对象中，并传给调用对象。
2. 调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象。
3. 该对象执行命令。

在这三步骤中，分别有 3 个不同的主体：发送者、传递者和执行者。在实现过程中，需要特别关注。

### 应用场景

有时候需要向某些对象发送请求，但是又不知道请求的接受者是谁，更不知道被请求的操作是什么。此时，命令模式就是以一种松耦合的方式来设计程序。

#### 代码实现

`setCommand`方法为按钮指定了命令对象，命令对象为调用者（按钮）找到了接收者（MenuBar），并且执行了相关操作。而按钮本身并不需要关心接收者和接受操作。

```js
// 接受到命令，执行相关操作
const MenuBar = {
  refresh() {
    console.log("刷新菜单页面");
  }
};

// 命令对象，execute方法就是执行相关命令
const RefreshMenuBarCommand = receiver => {
  return {
    execute() {
      receiver.refresh();
    }
  };
};

// 为按钮对象指定对应的 对象
const setCommand = (button, command) => {
  button.onclick = () => {
    command.execute();
  };
};

let refreshMenuBarCommand = RefreshMenuBarCommand(MenuBar);
let button = document.querySelector("button");
setCommand(button, refreshMenuBarCommand);
```

### <a name="组合模式">组合模式</a>

#### 描述

> 组合模式，将对象组合成树形结构以表示“部分-整体”的层次结构。

1. 用小的子对象构造更大的父对象，而这些子对象也由更小的子对象构成

2. 单个对象和组合对象对于用户暴露的接口具有一致性，而同种接口不同表现形式亦体现了多态性

### 应用场景

组合模式可以在需要针对“树形结构”进行操作的应用中使用，例如扫描文件夹、渲染网站导航结构等等。

#### 代码实现

这里用代码模拟文件扫描功能，封装了File和Folder两个类。在组合模式下，用户可以向Folder类嵌套File或者Folder来模拟真实的“文件目录”的树结构。
同时，两个类都对外提供了scan接口，File下的scan是扫描文件，Folder下的scan是调用子文件夹和子文件的scan方法。整个过程采用的是深度优先。

```js
// 文件类
class File {
  constructor(name) {
    this.name = name || "File";
  }

  add() {
    throw new Error("文件类下面不能添加文件");
  }

  scan() {
    console.log("扫描文件: " + this.name);
  }
}

// 文件夹类
class Folder {
  constructor(name) {
    this.name = name || "Folder";
    this.files = [];
  }

  add(file) {
    this.files.push(file);
  }

  scan() {
    console.log("扫描文件夹: " + this.name);
    for (let file of this.files) {
      file.scan();
    }
  }
}

let home = new Folder("用户根目录");

let folder1 = new Folder("第一个文件夹"),
  folder2 = new Folder("第二个文件夹");

let file1 = new File("1号文件"),
  file2 = new File("2号文件"),
  file3 = new File("3号文件");

// 将文件添加到对应文件夹中
folder1.add(file1);

folder2.add(file2);
folder2.add(file3);

// 将文件夹添加到更高级的目录文件夹中
home.add(folder1);
home.add(folder2);

// 扫描目录文件夹
home.scan();
```

最终输出结果:

```shell
扫描文件夹: 用户根目录
扫描文件夹: 第一个文件夹
扫描文件: 1号文件
扫描文件夹: 第二个文件夹
扫描文件: 2号文件
扫描文件: 3号文件
```

### <a name="享元模式">享元模式</a>

#### 描述

> 享元模式：运用共享技术来减少创建对象的数量，从而减少内存占用、提高性能。

1. 享元模式提醒我们将一个对象的属性划分为内部和外部状态。
    1. 内部状态：可以被对象集合共享，通常不会改变
    2. 外部状态：根据应用场景经常改变
2. 享元模式是利用时间换取空间的优化模式。

### 应用场景

享元模式虽然名字听起来比较高深，但是实际使用非常容易：只要是需要大量创建重复的类的代码块，均可以使用享元模式抽离内部/外部状态，减少重复类的创建。

为了显示它的强大，下面的代码是简单地实现了大家耳熟能详的“对象池”，以彰显这种设计模式的魅力。

#### 代码实现

这里利用javascript实现了一个“通用对象池”类--ObjectPool。这个类管理一个装载空闲对象的数组，如果外部需要一个对象，直接从对象池中获取，而不是通过new操作。

对象池可以大量减少重复创建相同的对象，从而节省了系统内存，提高运行效率。

为了形象说明“享元模式”在“对象池”实现和应用，特别准备了模拟了File类，并且模拟了“文件下载”操作。

通过阅读下方代码可以发现：对于File类，内部状态是pool属性和download方法；外部状态是name和src(文件名和文件链接)。借助对象池，实现了File类的复用。

```js
// 对象池
class ObjectPool {
  constructor() {
    this._pool = []; //
  }

  // 创建对象
  create(Obj) {
    return this._pool.length === 0
      ? new Obj(this) // 对象池中没有空闲对象，则创建一个新的对象
      : this._pool.shift(); // 对象池中有空闲对象，直接取出，无需再次创建
  }

  // 对象回收
  recover(obj) {
    return this._pool.push(obj);
  }

  // 对象池大小
  size() {
    return this._pool.length;
  }
}

// 模拟文件对象
class File {
  constructor(pool) {
    this.pool = pool;
  }

  // 模拟下载操作
  download() {
    console.log(`+ 从 ${this.src} 开始下载 ${this.name}`);
    setTimeout(() => {
      console.log(`- ${this.name} 下载完毕`); // 下载完毕后, 将对象重新放入对象池
      this.pool.recover(this);
    }, 100);
  }
}

/****************** 以下是测试函数 **********************/

let objPool = new ObjectPool();

let file1 = objPool.create(File);
file1.name = "文件1";
file1.src = "https://download1.com";
file1.download();

let file2 = objPool.create(File);
file2.name = "文件2";
file2.src = "https://download2.com";
file2.download();

setTimeout(() => {
  let file3 = objPool.create(File);
  file3.name = "文件3";
  file3.src = "https://download3.com";
  file3.download();
}, 200);

setTimeout(
  () =>
    console.log(
      `${"*".repeat(50)}\n下载了3个文件，但其实只创建了${objPool.size()}个对象`
    ),
  1000
);
```

输出结果如下：

```shell
+ 从 https://download1.com 开始下载 文件1
+ 从 https://download2.com 开始下载 文件2
- 文件1 下载完毕
- 文件2 下载完毕
+ 从 https://download3.com 开始下载 文件3
- 文件3 下载完毕
**************************************************
下载了3个文件，但其实只创建了2个对象
```

### <a name="责任链模式">责任链模式</a>

#### 描述

>责任链模式：多个对象均有机会处理请求，从而解除发送者和接受者之间的耦合关系。这些对象连接成为链式结构，每个节点转发请求，直到有对象处理请求为止。

其核心就是：请求者不必知道是谁哪个节点对象处理的请求。如果当前不符合终止条件，那么把请求转发给下一个节点处理。

而当需求具有“传递”的性质时（代码中其中一种体现就是：多个if、else if、else if、else嵌套），就可以考虑将每个分支拆分成一个节点对象，拼接成为责任链。

### 优点与代价

1. 优点
    1. 可以根据需求变动，任意向责任链中添加 / 删除节点对象
    2. 没有固定的“开始节点”，可以从任意节点开始
2. 代价：责任链最大的代价就是每个节点带来的多余消耗。当责任链过长，很多节点只有传递的作用，而不是真正地处理逻辑。

#### 代码实现

为了方便演示，模拟常见的“日志打印”场景。模拟了 3 种级别的日志输出：

`LogHandler`: 普通日志

`WarnHandler`：警告日志

`ErrorHandler`：错误日志

首先我们会构造“责任链”：`LogHandler` -> `WarnHandler` -> `ErrorHandler`。`LogHandler`作为链的开始节点。
如果是普通日志，那么就由 `LogHandler` 处理，停止传播；如果是 `Warn` 级别的日志，那么 `LogHandler` 就会自动向下传递，`WarnHandler` 接收到并且处理，停止传播；`Error` 级别日志同理。

```js
class Handler {
  constructor() {
    this.next = null;
  }

  setNext(handler) {
    this.next = handler;
  }
}

class LogHandler extends Handler {
  constructor(...props) {
    super(...props);
    this.name = "log";
  }

  handle(level, msg) {
    if (level === this.name) {
      console.log(`LOG: ${msg}`);
      return;
    }
    this.next && this.next.handle(...arguments);
  }
}

class WarnHandler extends Handler {
  constructor(...props) {
    super(...props);
    this.name = "warn";
  }

  handle(level, msg) {
    if (level === this.name) {
      console.log(`WARN: ${msg}`);
      return;
    }
    this.next && this.next.handle(...arguments);
  }
}

class ErrorHandler extends Handler {
  constructor(...props) {
    super(...props);
    this.name = "error";
  }

  handle(level, msg) {
    if (level === this.name) {
      console.log(`ERROR: ${msg}`);
      return;
    }
    this.next && this.next.handle(...arguments);
  }
}

/******************以下是测试代码******************/

let logHandler = new LogHandler();
let warnHandler = new WarnHandler();
let errorHandler = new ErrorHandler();

// 设置下一个处理的节点
logHandler.setNext(warnHandler);
warnHandler.setNext(errorHandler);

logHandler.handle("error", "Some error occur");

```

### <a name="装饰者模式">装饰者模式</a>

#### 描述

> 装饰者模式：在不改变对象自身的基础上，动态地添加功能代码。

根据描述，装饰者显然比继承等方式更灵活，而且不污染原来的代码，代码逻辑松耦合。

### 应用场景

装饰者模式由于松耦合，多用于一开始不确定对象的功能、或者对象功能经常变动的时候。 尤其是在参数检查、参数拦截等场景。

#### 代码实现

ES6的装饰器语法规范只是在“提案阶段”，而且不能装饰普通函数或者箭头函数。

下面的代码，`addDecorator`可以为指定函数增加装饰器。

其中，装饰器的触发可以在函数运行之前，也可以在函数运行之后。

注意：装饰器需要保存函数的运行结果，并且返回。

```js
const addDecorator = (fn, before, after) => {
  let isFn = fn => typeof fn === "function";

  if (!isFn(fn)) {
    return () => {};
  }

  return (...args) => {
    let result;
    // 按照顺序执行“装饰函数”
    isFn(before) && before(...args);
    // 保存返回函数结果
    isFn(fn) && (result = fn(...args));
    isFn(after) && after(...args);
    // 最后返回结果
    return result;
  };
};

/******************以下是测试代码******************/

const beforeHello = (...args) => {
  console.log(`Before Hello, args are ${args}`);
};

const hello = (name = "user") => {
  console.log(`Hello, ${name}`);
  return name;
};

const afterHello = (...args) => {
  console.log(`After Hello, args are ${args}`);
};

const wrappedHello = addDecorator(hello, beforeHello, afterHello);

let result = wrappedHello("godbmw.com");
console.log(result);
```

输出结果：

```shell
Before Hello, args are godbmw.com
Hello, godbmw.com
After Hello, args are godbmw.com
godbmw.com
```


