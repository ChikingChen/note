# HTML 通信

## 同域跨文档通信

使用iframe标签通信，iframe可以嵌套页面并该页面可以被访问window对象

## 跨域跨文档通信

- window.postMessage

  允许**window对象（利用对象的）**之间的通信，包括页面和弹窗之间，页面之间以及页面与子页面之间

  window.postMessage()会发送一个MessageEvent消息，

  ```js
  otherWindow.postMessage(message, targetOrigin, [transfer])
  ```

  - otherWindow

    目标窗口引用

  - message（发送方保留数据）

    将要发送的数据，自动序列化无需特殊处理

  - targetOrigin

    指明接收消息的窗口，两种选择：*或者一个URL，要求目标窗口**协议**、**主机地址**以及**端口**都要匹配才会发送

  - transfer（发送方不保留数据）

    一串和message同时传递的transferable对象

    发送前对象的所有权归属消息发送方，发送后对象的所有权归属消息接收方，消息发送方不再保存对象

- [window.addEventListener("message", function name(), false)](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#signal)

  利用如上函数可以监听别的窗口发出的消息

  ```js
  window.addEventListener("message", receiveMessage, false);
  function receiveMessage(event) {
    // For Chrome, the origin property is in the event.originalEvent
    // object.
    // 这里不准确，chrome 没有这个属性
    // var origin = event.origin || event.originalEvent.origin;
    var origin = event.origin;
    if (origin !== "http://example.org:8080") return;
  
    // ...
  }
  ```

  event的属性：

  - data

    从其他window发送的对象

  - origin

    发送方窗口的origin

  - source

    对发送消息窗口对象的引用

  监听器的格式：

  ```js
  addEventListener(type, listener);
  addEventListener(type, listener, options);
  addEventListener(type, listener, useCapture);
  ```

  - type

    监听的事件类型

  - listener

    回调函数或者实现了EventListener接口的对象

  - options

    指定有关listener属性的可选参数对象

    - capture
    - once
    - passive

  - useCapture

    

  

  



