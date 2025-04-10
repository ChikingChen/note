# asnyc

- [同步](https://developer.mozilla.org/zh-CN/docs/Glossary/Synchronous)

  双方实时反馈，一者在发出信息后，另一者立刻回复

  程序开发领域：一条代码执行完成后立刻执行第二条代码

- [异步](https://developer.mozilla.org/zh-CN/docs/Glossary/Synchronous)

  当前这条代码和先前那条代码一起执行，当前代码不需要等待后面代码的完成

## [promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

promise对象：异步操作最终完成以及结果值

允许将处理程序与异步操作**最终成功值和失败原因**关联起来，异步方法不会返回最终值而是返回一个promise

promise的状态：**待定**（初始），**已兑现**（操作成功），**已拒绝**（操作失败）

后两者任意一者发生时，使用promise的then方法串联的处理程序将被调用

后两者统称为**已敲定**

## await

await为等待一个promise兑现并且获取它兑现之后的值

## async function

较为方便地定义异步函数

两种异步

```js
function resolveAfter2Seconds(x) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

// 赋值给变量的异步函数表达式
const add = async function (x) {
  const a = await resolveAfter2Seconds(20);
  const b = await resolveAfter2Seconds(30);
  return x + a + b;
};

add(10).then((v) => {
  console.log(v); // 4 秒后打印 60
});
```



```js
(async function (x) {
  const p1 = resolveAfter2Seconds(20);
  const p2 = resolveAfter2Seconds(30);
  return x + (await p1) + (await p2);
})(10).then((v) => {
  console.log(v); // 2 秒后打印 60
});
```

- 箭头函数

  类似于C++中的lambda函数

  ```js
  () => {
    resolve(x);
  }
  ```

- resolve

  标记异步操作成功完成的回调函数

  resolve(x)将改变promise的状态，并将x作为promise的成功结果

  

