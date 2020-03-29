## Vue常见的问题

### 什么是MVVM

MVVM全程是Model-View-ViewModel，是一种分层的设计思想，Model表示数据层，View表示视图层。我们通过ViewModel层来实现Model层和View的相互同步的操作。

Vue是典型的MVVM框架。



#### 响应式原理

Vue使用`defineProperty`来实现，同步数据和视图的功能，这也就是我们所说的ViewModel层。

Vue3之后开始改为Proxy，主要是因为defineProperty必须要定义到具体属性，所以对于动态添加的属性值，则无法监听到（典型的就是数组）。

所以，defineProperty首先需要深度遍历我们的对象，对每个属性都加上getter/setter。

而Proxy则不用。



### Vue生命周期

可分为：

1. 创建前后

   beforeCreate: 创建Vue对象，事件&生命周期初始化完成

   created: 数据的注入和校验完成（可访问data属性）

2. 挂载前后

   beforeMount: 模板编译完成

   mounted: 创建vm.$el完成，可通过el访问元素

3. 更新前后

   beforeUpdate: 数据有修改则触发此函数

   updated：完成更新

4. 销毁前后

   beforeDestory: 销毁前调用

   destoryed: 解除绑定，销毁子组件以及事件监听器

### Vue watcher

观察者模式？发布订阅模式？

### 其他问题

+ 父子组件渲染时的生命周期

  应该是在编译模版那一步完成子组件的解析

+ 父子组件的通信问题

  1. 自定义事件
  2. event bus
  3. vuex

