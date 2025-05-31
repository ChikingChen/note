# django 后台

django数据库默认使用SQLite，如果要使用别的数据库要安装合适的数据库构建并且修改settings中的default参数（ENGIEN，NAME）

- ENGIEN

  mysql使用django.db.backends.mysql

- NAME

  数据库名称

- 别的参数

  详细查看[django文档](https://docs.djangoproject.com/en/5.0/ref/settings/#std-setting-DATABASES)

如果要在应用中使用数据表需要先在INSTALLED_APPS中注册app，运行migrate命令时候将在INSTALLED_APPS中查找注册的app而后对app中的数据表采用migrate命令

假设有个项目的名称为polls，我们就要将项目下apps文件中的config文件注册到INSTALLED_APPS列表中，即polls.apps.PollsConfig

## shell

使用python manage.py shell命令能够进入python shell

python shell中能够运行python内容进而对现有数据表增删改查

## view

django中web页面由view函数表达

## url

django中允许如下的url形式

```F#
/newsarchive/<year>/<month>
```

我们能够编写如下的url映射

```python
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path("", views.index, name="index"),
    # ex: /polls/5/
    path("<int:question_id>/", views.detail, name="detail"),
    # ex: /polls/5/results/
    path("<int:question_id>/results/", views.results, name="results"),
    # ex: /polls/5/vote/
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```

在如上的url映射中我们将需要传递的参数写入了url内部，在views函数中作为参数取用

```python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)


def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)


def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

其中request是一个HttpRequest对象，question_id是url中的int变量

## Http内容

在views中编写http相对比较困难，解决办法是利用django的模板系统分离python和模板设计

在项目下创建一个templates文件夹，使用loader函数引用

```python
from django.template import loader
```

### render函数

```python
render(request, "polls/index.html", context)
```

- requst

  views函数的http请求

- "polls/index.html"

  需要渲染的html文件

- context

  字典对象，对html文件中的变量进行替换

返回一个HttpResponse对象