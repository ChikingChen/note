# HTML 循环

HTML中没有类似v-for的形式，因此动态表格将使用js来实现

以下为实例代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>列表渲染示例</title>
</head>
<body>

<ul id="example-list">
  <!-- 通过JavaScript动态插入列表项 -->
</ul>

<script>
// 假设我们有一个数组
var items = [
  '苹果',
  '香蕉',
  '樱桃'
];

// 获取列表元素
var list = document.getElementById('example-list');

// 使用循环来遍历数组
for (var i = 0; i < items.length; i++) {
  // 创建新的列表项（li元素）
  var listItem = document.createElement('li');
  // 设置列表项的文本
  listItem.textContent = items[i];
  // 将列表项添加到列表中
  list.appendChild(listItem);
}
</script>

</body>
</html>
```

使用document.createElement来创建列表项，而后使用appendChild来将列表项加入列表中，也可使用列表项的API修改列表项的属性