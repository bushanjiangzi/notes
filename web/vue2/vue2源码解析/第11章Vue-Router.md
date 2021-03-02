## 注册路由

- Vue 编写插件时通常要提供静态 install 方法

- Vue-Router 的 install 方法会给每一个组件注入 beforeCreated 和 destoryed 钩子函数，在 beforeCreated 做一些私有属性定义和路由初始化工作。

## VueRouter 对象

- 路由初始化的时机是在组件初始化阶段，执行到 beforeCreate 钩子函数的时候会执行 router.init 方法。然后又会执行 history.transitionTo 方法做路由过渡

## matcher

- createMatcher 的初始化就是根据路由的配置描述创建映射表，包括路径、名称到路由 record 的映射关系。

- match 会根据传入的位置和路径计算出新的位置，并匹配到对应的路由 record，然后根据新的位置和 record 创建新的路径并返回。
