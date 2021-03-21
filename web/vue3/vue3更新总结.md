## Vue Function-based API RFC 
  
  - [Vue Function-based API RFC](https://zhuanlan.zhihu.com/p/68477600)

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
