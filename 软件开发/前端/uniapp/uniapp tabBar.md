# uniapp tabBar

## tabBar的初始化

- 在项目中创建四个页面

- 在pages.json中创建一个tabBar对象

  如下图

  ![1](.\1.png)

## 跳转到tabBar

```js
uni.switchTab({
	url: "/pages/appointment/appointment"
})
```

## [tabBar样式自定义](https://www.jb51.net/article/280653.htm)

### 大致思路

通过自定义view来实现tabbar，点击不同的tab显示不同的组件来模拟原生tabbar

使用easycom模式解决项目中tab数量和名称不确定的问题，提前设置组件个数和名称

- easycom

  vue需要安装、引用、注册三个步骤才能使用组件，uniapp中的easycom精简为一步（即直接使用）

  - 路径规范
    1. 安装在项目根目录的components目录下，并符合`components/组件名称/组件名称.vue`
    2. 安装在uni_modules下，路径为`uni_modules/插件ID/components/组件名称/组件名称.vue`

### 实现

1. 新建一个components文件夹并在下面新建一个文件夹，其中新建一个vue组件和一个js文件

   vue组件作为引用，js文件做配置

   ![2](.\2.png)

2. 新建tab组件，几个组件就在components文件夹下新建几个文件夹

   ![3](.\3.png)

3. 分别编写tab以及tabBar

   

