# vuex

**集中式**存储管理应用的所有组件的状态

## 状态自管理应用

- 状态，vue变量
- 视图，模板
- 操作，函数

把组件的共享状态抽取出来，全局单例模式管理，**组件树**构成了一个**巨大的“视图”**，任何组件都能获取状态或者触发行为

## 仓库

Vuex的核心——store（仓库）

容器，包含着应用中大部分的状态

- 和全局对象有两点不同
  1. 响应式
  2. 不能直接改变store中的状态，提交mutation来改变

通过state以及mutations定义store

```javascript
import { createApp } from 'vue'
import { createStore } from 'vuex'

// 创建一个新的 store 实例
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

const app = createApp({ /* 根组件 */ })

// 将 store 实例作为插件安装
app.use(store)
```

使用commit来调用函数

```javascript
store.commit('increment')

console.log(store.state.count) // -> 1
```

使用this.$store调用被定义的store

```JavaScript
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```

## State

单一状态树——一个状态包含了全部的应用层级状态，唯一数据源，每个应用只包含一个store