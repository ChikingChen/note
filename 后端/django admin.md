# django admin

## ModelAdmin

管理页面的模型

```python
@admin.register(AccountTable)
class AccountTableAdmin(admin.ModelAdmin):
    fields = ['nickname']
```

使用@admin.register(AccountTable)来注册页面同时将ACcountTableAdmin这个ModelAdmin子类指向AccountTable表格的管理页面

- exclude

  表格中不显示的属性，是一个list

- fields

  在添加修改界面中现实的属性

- actions

  批量操作

- list_display

  总体页面上的展示内容

## Admin actions

django的工作方式是选择一个对象然后改变它， 对多个应用同时操作不方便

可以编写django actions来完成这个功能，在数据表的右上角

### action functions

批量操作数据表的函数，需要三个参数

- ModelAdmin

  当下的显示界面对象

- HttpRequest

  后台的Http请求

- QuerySet

  用户选择的一系列对象

```python
def make_published(modeladmin, request, queryset):
    queryset.update(status="p")
```

可以使用@admin.action(description="XXX")装饰器来描述这个函数（记载后台显示界面中显示）

```python
@admin.action(description="Mark selected stories as published")
def make_published(modeladmin, request, queryset):
    queryset.update(status="p")
```

将函数放入名为actions的list中进行注册

```python
@admin.action(description="Mark selected stories as published")
def make_published(modeladmin, request, queryset):
    queryset.update(status="p")


class ArticleAdmin(admin.ModelAdmin):
    list_display = ["title", "status"]
    ordering = ["title"]
    actions = [make_published]
```

