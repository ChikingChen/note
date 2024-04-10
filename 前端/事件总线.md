# [EventBus](https://blog.csdn.net/shanghai597/article/details/130965196)

事件总线，使用EventBus来作为沟通桥梁的概念，所用组件公用相同的事件中心

缺点：维护困难

## 初始化

在event-bus.js中初始化

```js
import Vue from 'vue'
export const EventBus = new Vue()
```

在main.js中初始化

```js
Vue.prototype.$EventBus = new Vue()
```

## 发送和接收

- 发送

  ```js
  EventBus.$emit('message', 'content')
  ```

  - message：事件名称
  - content：事件参数

- 接收（事件监听）

  ```js
  EventBus.$on('message', (parameter) => { function; })
  ```

  - message：事件名称
  - parameter：事件参数

## 存在的问题

- 某页刷新之后相关的EventBus会被移除
- 业务反复操作的页面EventBus会触发很多次