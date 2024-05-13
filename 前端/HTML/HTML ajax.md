# [HTML ajax](https://www.runoob.com/ajax/ajax-xmlhttprequest-onreadystatechange.html)

## 创建 XMLHttpRequest 对象

```js
variable=new XMLHttpRequest();
```

## XHR 请求

```js
xmlhttp.open("POST","/try/ajax/demo_post.php",true);
// 请求方法 文件url 是否异步
xmlhttp.send();
```

使用ajax必须设置为异步

## XHR 响应

- responseText

  服务器响应为字符串形式，使用此属性读取响应内容

- responseXML

  服务器响应为XML形式，使用此属性读取响应内容

## onreadystatechange事件

标记ajax在任意阶段应该做的事情（等待，请求完成等等）

```js
xmlhttp.onreadystatechange=function()
{
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
```

- onreadystatechange

  函数名

- readyState

  - 0: 请求未初始化
  - 1: 服务器连接已建立
  - 2: 请求服务器已接收
  - 3: 请求服务器处理中
  - 4: 请求已完成，且响应已就绪

- status

  200: "OK"
  404: 未找到页面

# getResponseHeader() 方法

返回一个包含HTTP头特定值的字符串

如果要返回HTTP头的全部值使用getAllResponseHeaders()将返回一整个HTTP头文件

## 一个例子

```js
function loadXMLDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
    myFunction(this);
    }
  };
  // 定义完成阶段的函数
  xhttp.open("GET", "cd_catalog.xml", true);
  // 定义请求
  xhttp.send();
  //发送请求
}
function myFunction(xml) {
  var i;
  var xmlDoc = xml.responseXML;
  var table="<tr><th>Artist</th><th>Title</th></tr>";
  var x = xmlDoc.getElementsByTagName("CD");
  for (i = 0; i <x.length; i++) { 
    table += "<tr><td>" +
    x[i].getElementsByTagName("ARTIST")[0].childNodes[0].nodeValue +
    "</td><td>" +
    x[i].getElementsByTagName("TITLE")[0].childNodes[0].nodeValue + 
    "</td></tr>";
  }
  document.getElementById("demo").innerHTML = table;
}
```

