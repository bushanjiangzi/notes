## Vue Function-based API RFC

  - [Vue Function-based API RFC](https://zhuanlan.zhihu.com/p/68477600)

  - vue3.0将 2.x 中与组件逻辑相关的选项以 API 函数的形式重新设计动机在于：组件 API 设计所面对的核心问题之一就是如何组织逻辑，以及如何在多个组件之间抽取和复用逻辑。基于 Vue 2.x 目前的 API 我们有一些常见的逻辑复用模式，但都或多或少存在一些问题。这些模式包括：Mixins；高阶组件 (Higher-order Components, aka HOCs)；Renderless Components (基于 scoped slots / 作用域插槽封装逻辑的组件）

  - 以上这些模式存在以下问题：

  1. 模版中的数据来源不清晰。举例来说，当一个组件中使用了多个 mixin 的时候，光看模版会很难分清一个属性到底是来自哪一个 mixin。HOC 也有类似的问题。
  2. 命名空间冲突。由不同开发者开发的 mixin 无法保证不会正好用到一样的属性或是方法名。HOC 在注入的 props 中也存在类似问题。
  3. 性能。HOC 和 Renderless Components 都需要额外的组件实例嵌套来封装逻辑，导致无谓的性能开销。

  - Function-based API 受 React Hooks 的启发，提供了一个全新的逻辑复用方案，且不存在上述问题。使用基于函数的 API，我们可以将相关联的代码抽取到一个 "composition function"（组合函数）中 —— 该函数封装了相关联的逻辑，并将需要暴露给组件的状态以响应式的数据源的方式返回出来。

## 响应式原理

[vue2 响应式原理请看](https://gitee.com/bushanjiangzi/notes/blob/master/web/vue2/vue2%E6%80%BB%E7%BB%93.md)

**Vue3.0 响应式原理核心步骤**
[详解](https://zhuanlan.zhihu.com/p/176813790)

- reactive(data)：创建响应式对象

- Proxy(data,handler)：Vue3.0 响应式相对于 Vue2.0 的最大区别在于用到了 es6 中的方法 Proxy。这个方法不需要循环遍历 data 的每个属性，对每个属性都做一遍响应式处理。而是直接代理了整个 data 对象，拦截这个对象所包含的所有属性的 get、set 方法。这么做的好处:

  1. 就是在我们动态为 data 添加一个属性时，不用做任何处理，这个属性就已经是响应式的了；
  2. 数组的任何操作也都可以触发响应。

  ```
  var obj = new Proxy({}, {
    get: function (target, key, receiver) {
      console.log(`getting ${key}!`);
      return Reflect.get(target, key, receiver);
    },
    set: function (target, key, value, receiver) {
      console.log(`setting ${key}!`);
      return Reflect.set(target, key, value, receiver);
    }
  });
  ```

- effect(fn)：上面的设计，其实已经解决了 Vue2.0 的前两个不足。而为了解决第三个不足， Vue3.0 提出了 effect 函数，调用 effect 执行某个函数时，首先会执行一次这个函数。之后每次 obj.name 修改时，执行一次该函数。

- Vue3.0 响应式框架在设计上，将视图渲染和数据响应式完全分离开来。将响应式核心方法 effect 从原有的 Watcher 中抽离。这样，当我们只需要监听数据响应某种逻辑回调(例如监听某个 text 属性的变化，对他进行正则校验等。)，而不需要更新视图的时候，完全可以只从 effect 触发，屏蔽左边的视图渲染。

## 重构 Virtual DOM

  - 模板编译时的优化，将一些静态节点编译成常量

  - slot优化，将slot编译为lazy函数，将slot的渲染的决定权交给子组件

  - 模板中内联事件的提取并重用（原本每次渲染都重新生成内联函数）

## 代码结构调整，更便于Tree shaking，使得体积更小

## 使用Typescript替换Flow
