# HTML

标签：单标签，双标签

\<slot /\> 单标签

\<ChildComp\>\</ChildComp\>双标签

## 文件结构

\<!DOCTYPE html\> 标明html文件开始（必须使用）

## HTML 根元素

\<html\>\</html\>

## HTML 标题

六级标题

```html
<h1></h1>
...
<h6></h6>
```

## HTML 段落

```php+HTML
<p></p>
```

其中放置html内容

### HTML 换行

```html
<br />
```

## HTML 链接

```html
<a href="http://www.4399.com"></a>
```

## HTML 图像

```html
<img src="PathToImg"/>
```

## HTML 主体

```html
<body>
    
</body>
```

## HTML 常用属性

- class

  元素的类名

- id

  元素的唯一id

- style

  元素行内样式

## HTML 水平线

```html
<hr />
```

## HTML 样式

使用html的style属性，使用CSS定义

### 外部样式表

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

在一个文件中编写样式而后引用（mystyle.css）

### 内部样式表

在head部分的style中定义样式

```html
<head>

<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
</head>
```

### 内联样式

```html
<p style="color: red; margin-left: 20px">
This is a paragraph
</p>
```

## HTML 内联元素 块元素

- 内联元素

  块元素中的子元素，显示不会自动换行

  如\<span\>元素可以作为块元素中的一个子元素，使用区别于父组件的属性

- 块元素

  页面的主要元素

## HTML JS

使用\<script\>定义客户端脚本，包含脚本语句，可以通过src指向外部脚本

可用作内容动态更改

