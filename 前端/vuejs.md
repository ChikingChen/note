# vuejs

## 计算属性

computed(func)

对现有变量计算而后返回经过加工的变量

## 模板引用

\<p ref='name'\>hello\</p\>

设置ref属性作为引用名称

const name = ref(null)

使用同名空引用作为模板引用名称

name.value

模板引用的具体对象

## watch

watch(num, func)

num变化的时候，调用func

## 箭头函数

(a1, a2, ...) => { ... }

无函数名，箭头前为参数，箭头后为函数体

## 列表渲染

\<li v-for"todo in todos"\>

## 条件渲染

\<h1 v-if="awesome"\>\</h1\>

\<h1 v-else\>\</h1\>

## 对象

ref(a) 引用名为a

## Props

将父组件的内容传入子组件

子组件

const props = defineProps({

  msg: String

})

使用动态绑定将父组件中的msg传入子组件

父组件

\<ChildComp :msg="greeting"/\>

## Emits

将子组件的内容传入父组件

const ... = defineEmits(['func'])

定义触发事件名称

emit(func, a1, ...)

func事件名称，a1,a2...时间参数

\<ChildComp @func = ""\>

## 插槽

\<ChildComp\>...\</ChildComp\>

将内容写入ChildComp组件之间传入子组件中

\<slot\>...</slot\>

将传入子组件的内容从slot组件之间传出，slot组件之间的内容为默认内容

\<ChildComp\>

子组件的内容放入父组件中

## 周期函数

import {'mounted'} from 'vue'

## provide/inject

可以将父组件可以跨级将内容传递给子组件

父组件使用provide将内容传递给注入名，子组件函数使用inject函数读取注入名的内容

- 依赖

  ```JavaScript
  app.provide('message', 'hello!')
  ```

- 注入

  ```JavaScript
  const message = inject('message')
  ```

