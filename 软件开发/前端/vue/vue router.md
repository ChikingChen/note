# 路由

## 客户端 vs 服务端路由

- 服务端路由：根据用户访问的URL路径返回不同的响应结果（后端）
- 客户端路由：动态获取新的页面（前端）

## 简单路由

动态组件实现简单路由

监听**hashchange**（addListener）或者使用**History API**更新组件

## 使用的组件

- router-link

  不重新加载页面的情况下处理url

- router-view

  显示与url对应的组件

## 使用js定义路由

- 定义路由组件

  ```javascript
  const Home = { template: '<div>Home</div>' }
  const About = { template: '<div>About</div>' }
  ```

- 定义路由

  ```JavaScript
  const routes = [
    { path: '/', component: Home },
    { path: '/about', component: About },
  ]
  ```

- 创建路由实例，提供了history模式的实现

  ```javascript
  const router = VueRouter.createRouter({
    // 4. 内部提供了 history 模式的实现。为了简单起见，我们在这里使用 hash 模式。
    history: VueRouter.createWebHashHistory(),
    routes, // `routes: routes` 的缩写
  })
  ```

- 创建并挂载

  ```javascript
  const app = Vue.createApp({})
  // 确保 _use_ 路由实例使
  // 整个应用支持路由。
  app.use(router)
  
  app.mount('#app')
  ```

