## store

- store 就是一个数据仓库，为了更方便的管理仓库，我们把一个大的 store 拆分成一些 modules，整个 modules 是一个树形结构。每个 module 又分别定义了 state，getters，mutations、actions，我们也通过递归遍历模块的方式完成了它们的初始化。

- Vuex 提供这些 API 都是方便我们在对 store 做各种操作来完成各种能力，尤其是 mapXXX 的设计，让我们在使用 API 的时候更加方便，这也是我们在设计一些 JavaScript 库的时候，从 API 设计角度中应该学习的方向。

- Vuex 从设计上支持了插件，让我们很好地从外部追踪 store 内部的变化，Logger 插件在我们的开发阶段也提供了很好地指引作用。当然我们也可以自己去实现 Vuex 插件来帮助我们实现一些特定的需求
