# vue 子组件

## 全局组件

在Vue根实例中注册的组件，使用Vue.component方法，接收组件名称和选项对象

## 父组件

vue实例中通过components属性定义子组件

```vue
<template>
  <div>
    <h1>父组件</h1>
    <child-component></child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
}
</script>

```

