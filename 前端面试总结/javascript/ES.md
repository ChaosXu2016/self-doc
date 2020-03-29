## JavaScript相关知识

## 一些古老的需要注意的点

### 原型&原型链

### 作用域链

### 函数的几种声明方式和差异

###闭包

## 模块化

### ES6 Module & CommonJs

#### ES6 Module

```javascript
import { props } from 'module';

export const a = '1';

```

##### 特点

1. 编译时加载
2. 使用的是变量的引用
3. `import`导入的变量都是只读的
4. `ES6-module`使用严格模式
5. 按需加载

#### CommonJS

```javascript
const a = require('./b.js')

exports.name = 'a'
```

##### 特点

1. 运行时加载
2. 会在第一次加载的时候，执行目标js，并缓存结果
3. 不会按需加载，会一次加载整个文件

#### 循环加载

> 应该尽量避免循环加载

CommonJS，在遇到循环加载的时候，会暂停执行，直到将依赖的模块执行完了再接着执行当前的模块。

ES6类似，但是因为到出的只是引用，所以表现上可能和CommonJS有差别。

比如说如下代码：

```javascript
/**********es6***********/
// even.js
import { odd } from './odd'
export var counter = 0;
export function even(n) {
  counter++;
  return n === 0 || odd(n - 1);
}

// odd.js
import { even } from './even';
export function odd(n) {
  return n !== 0 && even(n - 1);
}
/**********es6***********/
/**********commonJs***********/
// even.js
var odd = require('./odd');
var counter = 0;
exports.counter = counter;
exports.even = function (n) {
  counter++;
  return n == 0 || odd(n - 1);
}

// odd.js
var even = require('./even').even;
module.exports = function (n) {
  return n != 0 && even(n - 1);
}
/**********commonJs***********/
```



Es6中，因为只是相当于导出一个接口，所以在`odd.js`执行到`even(n - 1)`的时候，实际上他会根据引用去找到`even.js`中的`even`函数

但是`CommonJS`则不然，因为`CommonJS`的`require`获取到的是缓存的值，而在执行到`var even = require('./even').even;`的时候`even`被赋值为`undefined`了，所以执行会报错。

这也是`CommonJS`和`ES6 Module`的本质区别导致的。



## Promise

### Event Loop

1. JS是单线程的
2. 为了避免主线程阻塞，提出了Event Loop
3. 首先JS会去执行所有的同步代码，然后再执行异步代码
4. 异步代码会放在一个队列里面，顺序执行
5. js分为两种异步队列：宏任务和微任务
6. 大体的是宏任务可以看作是一个微任务队列，一个异步队列可以看作是宏任务队列。
7. 执行的时候，依次执行宏任务 -> 宏任务中的同步代码 -> 宏任务中的微任务队列 -> 下一个宏任务
8. 宏任务有：setTimeout、setInterval、setImmediate、I/O、UI交互事件
9. 微任务：Promise、process.nextTick、MutaionObserver

主要的执行过程如下：

1. 执行同步代码
2. 再按照异步代码的插入顺序来执行异步代码

### Promise基本规范

Promise是一种异步解决方案，一个Promise包含三种状态`pending`、`fulfilled`和`rejected`。

+ prototype.then(resolved, rejected?)

  1. then方法返回一个新的Promise实例。
  2. resolved返回非Promise对象时，该返回结果作为下一次`then`函数的`resolved`入参

+ prototype.cache(rejected)

  1. 和`.then`一样，差别是只接受`rejected`的回调函数

+ Promise.all(Promise[])

  1. 返回一个Promise对象

  2. 当所有的Promise都执行完毕时，这个Promise才执行完毕

  3. 当所有的Promise全部都`fulfilled`了，这个Promise的状态才为`fulfilled`，否则为`rejected`

+ Promise.race(Promise[])

  1. 返回一个Promise对象
  2. 只要有一个Promise执行结束了，则该Promise执行结束

+ Promise.resolve(any)

  1. 返回一个状态为`fulfilled`的Promise

+ Promise.reject(any)

  1. 返回一个状态为`rejected`的Promise

  

### 手写一个简单的Promise的步骤



## Generator

### 基本语法

### 异步应用

### async函数

### 通过一个请求拦截器来比较和Promise的写法差异

### redux-saga



## Proxy

### 基本语法

### 对比getter&setter

### 应用



