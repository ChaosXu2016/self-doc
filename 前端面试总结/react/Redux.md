# Redux

## Redux

### 三大原则

+ 单一数据
+ state只读
+ reducer是纯函数

### action/reducer

+ Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。当应用规模越来越大时，建议使用单独的模块或文件来存放 action。
+ **Reducers** 指定了应用状态的变化如何响应 [actions](https://www.redux.org.cn/docs/basics/Actions.html) 并发送到 store 的，记住 actions 只是描述了*有事情发生了*这一事实，并没有描述应用如何更新 state。

### listener



## middleware

middleware 是指可以被嵌入在框架接收请求到产生响应过程之中的代码。例如，Express 或者 Koa 的 middleware 可以完成添加 CORS headers、记录日志、内容压缩等工作。middleware 最优秀的特性就是可以被链式组合。你可以在一个项目中使用多个独立的第三方 middleware。

我们组合起来看一下redux关于middleware这块的代码

```javascript
// createStore
export default function createStore(reducer, preloadedState, enhancer) {
  ...
  if (typeof enhancer !== 'undefined') {
    if (typeof enhancer !== 'function') {
      throw new Error('Expected the enhancer to be a function.')
    }
    return enhancer(createStore)(reducer, preloadedState)
  }
  ...
  return {
    dispatch,
    subscribe,
    getState,
    replaceReducer,
    [$$observable]: observable
  }
}

```

上面的代码我们可以看到，`createStore`在创建`store`的时候，会判断是否有`enhancer`。如果有，则执行`enhancer(createStore)(reducer, preloadedState)`。

而这里的`enhancer`是通过`applyMiddleware`来创建的。所以我们可以看一下`applyMiddleware`的代码，看看这个`enhancer`到底干了什么事

```javascript
import compose from './compose'

export default function applyMiddleware(...middlewares) {
  return createStore => (...args) => {
    const store = createStore(...args)
    let dispatch = () => {
      throw new Error(
        'Dispatching while constructing your middleware is not allowed. ' +
          'Other middleware would not be applied to this dispatch.'
      )
    }
    
    const middlewareAPI = {
      getState: store.getState,
      dispatch: (...args) => dispatch(...args)
    }
    const chain = middlewares.map(middleware => middleware(middlewareAPI))
    dispatch = compose(...chain)(store.dispatch)

    return {
      ...store,
      dispatch
    }
  }
}

```

代码中可以看到，我们复写了`createStore`的`dispatch`函数。`dispatch = compose(...chain)(store.dispatch)`。

我们可以简单的看一下`compose`函数：

```javascript
/**
 * Composes single-argument functions from right to left. The rightmost
 * function can take multiple arguments as it provides the signature for
 * the resulting composite function.
 *
 * @param {...Function} funcs The functions to compose.
 * @returns {Function} A function obtained by composing the argument functions
 * from right to left. For example, compose(f, g, h) is identical to doing
 * (...args) => f(g(h(...args))).
 */

export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

这个函数很简单，从注释就可以看到，这个函数完成的是从左到右，依次的以上一个函数的结果作为下一个函数的输出。

因此，我们可以梳理出来`redux`的中间件的标准：

1. redux的middleware是拦截`store`的dispatch，在执行原dispatch之前，执行一些自定义的逻辑
2. middleware允许链式调用。具体的执行逻辑是从右至左，我们可以利用这个特点，拆分我们的middleware
3. 如果我们注册了middleware，那么我们在之后每次dispatch的时候都会把所有middleware都走一遍

我们还可以得出middleware的普遍写法（官方文档有关于这种写法的[演进过程](https://www.redux.org.cn/docs/advanced/Middleware.html)，感兴趣的可以看一下）：

```javascript
const middleware = store => next => action => {
  next(action)
}
```

#### redux-thunk

Redux-thunk非常的短小精悍，因此，我们下面贴出他的源码：

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;

```

从源码中我们可以看出，thunk middleware就做了一件事：**加入action是函数，则执行这个函数**。通过这个我们可以办到很多事，比如在`action`函数内部我们去触发其他的`action`。比如说处理请求结果：

1. 由action函数触发请求
2. 在请求`success`的时候，触发`FETCH_SUCCESS`的action
3. 在请求`failure`的时候，触发`FETCH_FAILURE`的action

```javascript
const fetchData = (dispatch, getState) => {
  fetch('/service/data').then(res => {
    dispath({
      type: 'FETCH_SUCCESS',
      data: res
    })
  }).catch(error => {
    dispath({
      type: 'FETCH_FAILURE',
      error,
    })
  })
}
```

当然，我们可以用thunk函数做一些非异步的事，比如说处理一些action的副作用，保证操作的原子性。

#### redux-saga

redux-saga是redux的另一种异步解决方案，和thunk函数不同的是，saga并不是使用回调函数的方式来完成异步调用的，而是通过一个个effect来触发一个个task。

我们可以通过：`takeEvery`、`takeLatest`、`takeLeading`、`throttle`来决定我们触发saga函数的方式

可以通过`call`、`put`、`apply`、`cps`、`fork`、`spawn`、`join`、`cancel`、`selector`

