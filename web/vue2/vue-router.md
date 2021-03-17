## router.js
  ```
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  const Home = () => import('@/views/Home.vue')

  Vue.use(VueRouter)
  const originalPush = VueRouter.prototype.push
  VueRouter.prototype.push = function push(location) {
    return originalPush.call(this, location).catch((err) => err)
  }
  const routes = [
    {
      path: '/',
      redirect: '/menu',
      name: 'Home',
      component: Home,
      icon: 'iconicon-yuanchengshipin'
    },
    {
      path: '/menu',
      redirect: '/menu/index',
      name: 'Home',
      component: Home,
      isMenu: true,
      icon: 'iconicon-yuanchengshipin',
      children: [ // 子路由
        ...
      ]
    }
  ]

  const router = new VueRouter({
    mode: 'history', // 路由模式
    base: process.env.BASE_URL,
    routes
  })

  export default router
  ```
 - history模式在nginx下需要配置
 ```
  location / {
    try_files $uri $uri/ /index.html;
  }
 ```

 ## router实例说明
  - this.$router // 获取路由实例

  - this.$router.currentRoute // 获取当前路由

  - this.$router.options // 获取路由实例配置项(router.js中的配置)

 ## 路由跳转及参数传递
  - `router-link`方式
  ```
  <router-link  :to="{ name: 'Detail', params: { id: id }, query: { from: 'call' } }"></router-link>
  // 传参方式: params和query
  // params需要在router.js中配置
  {
    path: '/detail/:id',
    name: 'Detail',
    component: Detail
  }
  ```

  - this.router.push()方式
  ```
  this.$router.push({ name: 'Detail', params: { id: id }, query: { from: 'call' } } })
  ```

  - 在新窗口中打开
  ```
  let routeUrl = this.$router.resolve({
    name: 'Detail',
    params: { id: this.id },
    query: { from: 'call' }
  })
  window.open(routeUrl.href, '_blank')
  ```

  - 获取参数
  ```
  this.$route.params
  // response: { id: "88" }
  this.$route.query
  // response: { from: "call" }
  ```