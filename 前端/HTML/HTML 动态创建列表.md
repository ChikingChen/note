# HTML 动态创建列表

- 创建容器以及js引用

  ```h't'm
  <div id="clinicList"></div>
  ```

  ```js
  clinicList = document.getElementById('clinicList')
  ```

- 创建子组件并加入容器中

  ```js
  for (var i = 1; i <= 5; i++) {
    var listItem = document.createElement("li");
    listItem.textContent = "Item " + i;
    myList.appendChild(listItem);
  }
  ```

  - document.createElement("li")

    创建li类型的组件

  - listItem.textContent = "Item " + i;

    修改组件的属性

  - myList.appendChild(item)

    将创建的组件加入循环列表中