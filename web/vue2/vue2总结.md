## 响应式原理

- [参考](https://www.jianshu.com/p/d137fbdc06ff)

- ![原理图](https://gitee.com/bushanjiangzi/notes/blob/master/web/vue2/vue2%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90/img/%E5%93%8D%E5%BA%94%E5%BC%8F%E5%8E%9F%E7%90%86.png)

- 核心

  1. observe(data)：当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property；

  2. defineProperty(data,key,handler）：将遍历完的 property 使用 Object.defineProperty 把这些 property 全部转为 getter/setter。Object.defineProperty 是 ES5 中一个无法 shim 的特性，这也就是 Vue 不支持 IE8 以及更低版本浏览器的原因。getter/setter 就是做订阅-发布处理（依赖收集与派发更新），就是在属性被调用的使用，触发 get 代理函数，订阅调用该属性的组件(将组件存放到一个订阅者数组中进行保存，这里的 Dep.target 暂时可以理解为一个全局变量，代表着的是当前正在渲染的 Vue 组件)。 而在属性被修改时，触发 set 代理函数，在 set 代理函数里，通知订阅者数组里面的每一个订阅者（组件）进行视图更新。

  3. Dep：是一个依赖类，将它理解成一个订阅者。当对一个属性进行响应式处理的时候，就会实例化一个 Dep 实例，并将用到这个属性的组件全部存放在 subs 数组里。当属性被修改时，则通知 subs 数组里的所有组件进行更新。

  4. Watcher：getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 能够追踪依赖，在 property 被访问和修改时通知变更。每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。（等这些属性进行被修改时，就会通知这个 watcher，再次调用这个 watcher 里面的 render 函数，进行虚拟 dom 的 diff 和更新）

- vue2 响应式不足之处

  1. 动态添加响应式属性必须用 Vue.set

  2. 直接操作数组索引无法触发视图更新

  3. 数据的响应式处理和视图未完全解耦

## 全局 API

- Vue.use(plugins) 注册一个插件

  ```
  import Vue from 'vue'
  import Router from 'vue-router'

  Vue.use(VueRouter)
  ```

- Vue.directive()创建或者获取自定义指令

  ```
  // 注册(指令函数)
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })

  // 注册 (指令函数)
  Vue.directive('my-directive', function () {
    // 这里将会被 `bind` 和 `update` 调用
  })

  // getter，返回已注册的指令
  var myDirective = Vue.directive('my-directive')
  ```

- Vue.filter() 注册或者获取全局的过滤器
  **注册**

  ```
  //全局过滤器
  import Vue from 'vue';
  Vue.filter('formatString', function (value) {
    var msg = value.length > 10 ? value.slice(0,3): value;
    return msg;
  });
  ```

  **使用**

  ```
  //使用
  <template>
    <div id="app">
      {{msg | formatString}}
      {{student.name | formatString}}
      <ChildComponent v-bind:message="msg | formatString"></ChildComponent>
    </div>
  </template>
  ```

- Vue.nextTick([callback,context]) 在 DOM 更新之后调用回调函数

  ```
  //全局
  Vue.nextTick(() => {
    // DOM 更新了
  })
  // 局部
  this.$nextTick(() => {
    console.log("nextTick操作")
  })
  ```

- Vue.set(target, propertyName/index, value) or this.$set(target, propertyName/index, value)

  > 向响应式数据添加一个属性，并且保证该属性也是响应式的，且能够触发视图的更新。

  ```
  export default {
    name: 'App',
    data(){
      return {
        msg: "启动测试页面啦啦啦",
        student: {
          name: 1,
          age: 2,
        }
      }
    },
    mounted(){
      this.$set(this.student, "sex","女");
    }
  }
  ```

- Vue.delete( target, propertyName/index )

  > 删除一个对象的属性，如果属性是响应式的，确保删除属性并且更新视图

- Vue.mixin()混入

  > 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

  **局部**

  > 当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。比如，数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

  ```
  var mixin = {
    data: function () {
      return {
        message: 'hello',
        foo: 'abc'
      }
    }
  }

  new Vue({
    mixins: [mixin],
    data: function () {
      return {
        message: 'goodbye',
        bar: 'def'
      }
    },
    created: function () {
      console.log(this.$data)
      // => { message: "goodbye", foo: "abc", bar: "def" }
    }
  })
  ```

  **全局**

  ```
  // 为自定义的选项 'myOption' 注入一个处理器。
  Vue.mixin({
    created: function () {
      var myOption = this.$options.myOption
      if (myOption) {
        console.log(myOption)
      }
    }
  })

  new Vue({
    myOption: 'hello!'
  })
  // => "hello!"
  ```

- Vue.compile(template)

  > 参数： template {string}；在 render 函数中编译模板字符串。只在独立构建时有效

- Vue.observable( object )

  > 让一个对象可响应。Vue 内部会用它来处理 data 函数返回的对象

  > 返回的对象可以直接用于渲染函数和计算属性内，并且会在发生改变时触发相应的更新。也可以作为最小化的跨组件状态存储器，用于简单的场景：

  ```
  const state = Vue.observable({ count: 0 })
  const Demo = {
    render(h) {
      return h('button', {
        on: { click: () => { state.count++ }}
      }, `count is: ${state.count}`)
    }
  }
  ```
