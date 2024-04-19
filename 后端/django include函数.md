# django include/path函数

## include函数

django遇到include时候会截断匹配的url部分，将剩余的字符串发送到URLconf进一步处理

允许urlconfs更加符合restful思想，使得不同的应用/界面都能拥有自己的urlconfs，使得不同应用界面之间的继承关系相互独立，形成树状页面结构，解决页面过多是的单一的urlconfs过于臃肿

### 例子

![2](C:\Users\Chen Zhiyuan\Desktop\笔记\后端\2.png)

- 不使用include()结构

  - mysite下的urls

    ```python
    from django.urls import path, include
    
    urlpatterns = [
        path('', main.views,name='main'),
        path('page/1', page1.views,name='page_1'),
        path('page/2', page2.views,name='page_2'),
    ]
    ```

- 使用include()结构

  每个view目录下都存在一个urls文件

  - mysite下的urls.py

    ```python
    from django.urls import path,include
    
    urlpatterns = [
        path('', include('main.urls')),
    ]
    ```

  - main下的urls.py

    ```python
    from django.urls import path,include
    
    from . import views
    
    urlpatterns = [
        path('', views.main, name='main'),
        path('page/1', include('page1.urls')),
        path('page/2', include('page2.urls')),
    ]
    ```

  - page1下的urls.py

    ```python
    from django.urls import path
    
    from . import views
    
    urlpatterns = [
        path('', views.page1,name="page_1"),
    ]
    ```

  - page2下的urls.py

    ```python
    from django.urls import path
    
    from . import views
    
    urlpatterns = [
        path('', views.page2,name="page_2"),
    ]
    ```

### 总结

通过include我们能够构建起树形结构，path函数中放置view函数的地方倘若使用include函数即表示根据匹配的url转发到指定的文件夹中而后由指定的文件夹中的urls.py文件持续转发直至找到指定的views函数

可以认为include函数即指明了被转发的地址为文件或文件夹而非视图函数

## path函数

将一个**url**映射到一个**视图函数**或者**类**

```python
path(route, view, kwargs=None, name=None)
```

- route

  url路径

- view

  url被访问时候调用的视图函数或者基于类的视图

- kwargs

  字典，传递给视图函数的额外关键字参数

- name

  给url路由命名，在模板和视图中通过这个名字反向解析URL

  ```python
  urlpatterns = [
  	path("", views.index, name='index')
  ]
  ```

  可以在django模板中使用

  ```html
  {% url'index' %}
  ```

