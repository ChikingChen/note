# django 图片

django中的图片引用问题即为静态资源引用问题，解决方法如下

在项目文件夹下创建一个static文件夹，django将会在static文件夹中寻找静态文件（和templates相似）

在html中使用{% static %}标签即可指向静态文件夹

> 使用static标签之前需要使用load命令
>
> {% load static %}

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
{% load static %}
<!-- 导入static文件夹 -->
<img src="{% static 'img/test.webp' %}">
<!-- 引用static文件夹中的img/test.webp文件 -->
</body>
</html>
```

