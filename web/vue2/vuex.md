## new Vuex.Store
  ```
  // index.js
  import Vue from 'vue'
  import Vuex from 'vuex'
  import loginStore from './login'

  Vue.use(Vuex)
  const store = new Vuex.Store({
    state: {
      selectDay: ''
    },
    mutations: {
      setSelectDay(state, data) {
        state.selectDay = data
      }
    },
    modules: {
      login: loginStore
    },
    action: {}
  })
  export default store
  ```
  - login.js模块中
  ```
  const loginStore = {
    // login.js
    state: {
      loading: false,
    },
    mutations: {
      setLoading: (state, data) => {
        state.loading = data
      }
    },
    actions: {
      onLogin: async (context, payload) => {
        ...
        context.commit('setLoading', true)
      }
    },
    getters: {}
  }

  export default loginStore
  ```

## 触发mutations、actions及获取state

  - 触发mutations:`this.$store.commit('setSelectDay', '2021-03-17')`

  - 触发actions:`this.$store.dispatch('login/onLogin', { name: 'jiang' })`

  - 获取index.js中的state: `this.$store.state.selectDay`

  - 获取login.js中的state: `this.$store.state.login.loading`

## Mutation 需遵守 Vue 的响应规则

  - 既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

    1. 最好提前在你的 store 中初始化好所有所需属性。

    2. 当需要在对象上添加新属性时，你应该使用 `Vue.set(obj, 'newProp', 123)`, 或者以新对象替换老对象，如：`state.obj = { ...state.obj, newProp: 123 }`

## 使用常量替代 Mutation 事件类型

  - 使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：

  ```
  // mutation-types.js
  export const SOME_MUTATION = 'SOME_MUTATION'

  // store.js
  import Vuex from 'vuex'
  import { SOME_MUTATION } from './mutation-types'

  const store = new Vuex.Store({
    state: { ... },
    mutations: {
      // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
      [SOME_MUTATION] (state) {
        // mutate state
      }
    }
  })
  ```

## Mutation 必须是同步函数

- 我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

## mapMutations

  - 可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。

  ```
  import { mapMutations } from 'vuex'
  export default {
    // ...
    methods: {
      ...mapMutations([
        'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

        // `mapMutations` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
      ]),
      ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
      })
    }
  }
  ```

## mapActions

  - `...mapActions(['onLogin'])` // 将 `this.onLogin()` 映射为 `this.$store.dispatch('login/onLogin')`

  - `...mapActions({ login: 'onLogin' })` // 将 `this.login()` 映射为 `this.$store.dispatch('login/onLogin')`

## Actions 支持同样的载荷方式和对象方式进行分发：
  ```
  // 以载荷形式分发
  store.dispatch('incrementAsync', {
    amount: 10
  })

  // 以对象形式分发
  store.dispatch({
    type: 'incrementAsync',
    amount: 10
  })
  ```

## 涉及到调用异步 API 和分发多重 mutation
  ```
  actions: {
    checkout ({ commit, state }, products) {
      // 把当前购物车的物品备份起来
      const savedCartItems = [...state.cart.added]
      // 发出结账请求，然后乐观地清空购物车
      commit(types.CHECKOUT_REQUEST)
      // 购物 API 接受一个成功回调和一个失败回调
      shop.buyProducts(
        products,
        // 成功操作
        () => commit(types.CHECKOUT_SUCCESS),
        // 失败操作
        () => commit(types.CHECKOUT_FAILURE, savedCartItems)
      )
    }
  }
  ```

## getter

  - 有时候我们需要从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数：

  ```
  computed: {
    doneTodosCount () {
      return this.$store.state.todos.filter(todo => todo.done).length
    }
  }
  ```

  - 如果有多个组件需要用到此属性，我们要么复制这个函数，Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

  ```
  const store = new Vuex.Store({
    state: {
      todos: [
        { id: 1, text: '...', done: true },
        { id: 2, text: '...', done: false }
      ]
    },
    getters: {
      doneTodos: state => {
        return state.todos.filter(todo => todo.done)
      }
    }
  })
  ```

  - 访问，Getter 会暴露为 store.getters 对象

  `this.$store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]`

  - Getter 也可以接受其他 getter 作为第二个参数：
  ```
  getters: {
    // ...
    doneTodosCount: (state, getters) => {
      return getters.doneTodos.length
    }
  }
  // 获取
  this.$store.getters.doneTodosCount // -> 1
  ```

  - 也可以通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组进行查询时非常有用。注意，**getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果**
  ```
  getters: {
    // ...
    getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
    }
  }
  // store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
  ```

  - mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性
  ```
  import { mapGetters } from 'vuex'
  export default {
    // ...
    computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
      ...mapGetters([
        'doneTodosCount',
        'anotherGetter',
        // ...
      ])
    }
  }
  // 如果你想将一个 getter 属性另取一个名字，使用对象形式：
  ...mapGetters({
    // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
    doneCount: 'doneTodosCount'
  })
  ```


## Module
  - Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割
  ```
  const moduleA = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... },
    getters: { ... }
  }

  const moduleB = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... }
  }

  const store = new Vuex.Store({
    modules: {
      a: moduleA,
      b: moduleB
    }
  })

  store.state.a // -> moduleA 的状态
  store.state.b // -> moduleB 的状态
  ```

  ## 命名空间

    - 默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应.

    - 如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名.

    ```
    const store = new Vuex.Store({
      modules: {
        account: {
          namespaced: true,

          // 模块内容（module assets）
          state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
          getters: {
            isAdmin () { ... } // -> getters['account/isAdmin']
          },
          actions: {
            login () { ... } // -> dispatch('account/login')
          },
          mutations: {
            login () { ... } // -> commit('account/login')
          },

          // 嵌套模块
          modules: {
            // 继承父模块的命名空间
            myPage: {
              state: () => ({ ... }),
              getters: {
                profile () { ... } // -> getters['account/profile']
              }
            },

            // 进一步嵌套命名空间
            posts: {
              namespaced: true,

              state: () => ({ ... }),
              getters: {
                popular () { ... } // -> getters['account/posts/popular']
              }
            }
          }
        }
      }
    })
    ```