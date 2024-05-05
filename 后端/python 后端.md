## python 后端 Django

### 创建数据库

```
python manage.py makemigrations # 将模型转化为migration
python manage.py migrate # 将migration转化为数据表
```

### url

建立一个url和python回调函数之间的映射

> python回调函数：通过函数名称调用的函数
>
> 函数名中的函数指针，在特定条件下可以调用

```python
from django.urls import path

from . import views

urlpatterns = [
    path("articles/<int:year>/", views.year_archive),
    path("articles/<int:year>/<int:month>/", views.month_archive),
    path("articles/<int:year>/<int:month>/<int:pk>/", views.article_detail),
]
```

可以使用特殊格式来给出特定的回调函数

其中的\<int:year>\<int:month\>\<int:pk\>为特定格式，分别为year month以及pk变量赋值并填入回调函数变量

path函数中的回调函数必须要有httpresponse类型的返回值

### Models

django自带的**数据库**

一个model可以映射为一个数据表（from django.db import models）

```python
from django.db import models
class Person(models.Model): # 创建数据表
    first_name = models.CharField(max_length=30) # 对象中的成员即为数据表的表项
    last_name = models.CharField(max_length=30)
```

参考手册[Django官网](https://docs.djangoproject.com/en/5.0/topics/db/models/)

### 项目格式

- manage.py

  程序员与Django项目交互的媒介

- \_\_init\_\_.py

  指明python项目

- settings.py

  Django项目整体配置

- urls.py

  Django项目的url配置

  - path函数
  
    path(route, view)
  
    - route
  
      路由地址
  
    - view
  
      回调函数，必须使用include函数，除了admin.site.urls（默认存在）
  
      
  
    
