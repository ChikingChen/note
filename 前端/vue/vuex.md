# vuex

**集中式**存储管理应用的所有组件的状态

## vuex配置

配置步骤

- 创建store文件夹并在index.js中注册
- 在main.js中挂载

配置的具体内容

- main.js

  ```js
  import App from './App'
  import store from './store' // 从store文件夹中导入store
  
  // #ifndef VUE3
  import Vue from 'vue'
  import './uni.promisify.adaptor'
  Vue.config.productionTip = false
  Vue.prototype.$store = store // 在vue2中注册store
  App.mpType = 'app'
  const app = new Vue({
  	...App,
  	store // 在vue2中注册store
  })
  app.$mount()
  // #endif
  
  // #ifdef VUE3
  import { createSSRApp } from 'vue'
  export function createApp() {
  	const app = createSSRApp(App)
  	app.use(store) // 在vue3中注册store
  	return {
  		app
  	}
  }
  export const Account = '123'
  // #endif
  ```

- store/index.js

  ```js
  // 注册vuex
  // #ifndef VUE3
  import Vue from 'vue'
  import Vuex from 'vuex'
  Vue.use(Vuex)
  const store = new Vuex.Store({
  // #endif
  
  // #ifdef VUE3
  import { createStore } from 'vuex'
  const store = createStore({
  // #endif
  // store具体内容
  	state: {
  		Account: ""
  	},
  	mutations: {
  		login(state, Account) {
  			state.Account = Account
  		}
  	}
  })
  
  export default store
  
  ```

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
  state () { // 变量
    return {
      count: 0
    }
  },
  mutations: { // 方法
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

使用getStore、useStore调用被定义的store

## State

单一状态树——一个状态包含了全部的应用层级状态，唯一数据源，每个应用只包含一个store实例

store实例中读取状态的方式：在**计算属性**中返回某个值

```js
computed:{
	count(){
		return store.state.count
	}
}
```

### [mapState辅助函数](https://vuex.vuejs.org/zh/guide/state.html#mapstate-%E8%BE%85%E5%8A%A9%E5%87%BD%E6%95%B0)

替代**计算属性**，减少冗余

```js
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

## Mutation

更改Vuex的store中状态的唯一方法

### 如何调用mutation

```js
store.commit('increment')
```

### 传入额外的参数

```js
mutations: {
  increment (state, n) {
    state.count += n
  }
}

store.commit('increment', 10)
```

第二个参数可以是一个对象，增加可读性

