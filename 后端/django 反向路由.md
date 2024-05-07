# django 反向路由

使用一个标志信息来找到对应的路由

> 正向路由：路由指向视图函数
>
> 反向路由：标志信息找到路由

# django reverse函数

根据路由别名或者视图函数对象得到url

## 路由别名

```python
urlpatterns = [
	path('admin_note/', include('note.urls', namespace='admin_note')),
    path('index/', views.index, name='index')
]
```

转向路由使用namespace指定

普通路由使用name指定

## 在reverse函数中表示路由别名

```python
reverse('note:index', kwargs={'id': 100})
```

note指定namespace（是否只支持一级转发？？？）

index指定路由路径

kwargs使用字典指定路由路径中的变量

# django redirect函数

重定向函数，将一个url重定向到另一个url，属于django.shortcuts模块

