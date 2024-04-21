# django 数据库

django中的模型被django.db.models.Model的子类代表。

## Field

field是一个抽象类，即数据表中的属性

### 引入

```python
from django.db.models import CharField
```

### 类型

- CharField

  字符串属性

  - max_length

    字符串长度限制

  - primary_key

    设置主键

  - verbose_name

    设置查看名称

  - null

    能否为空

- DateField

  日期属性

  - auto_now_add

    添加时候自动设置为当前日期

- IntegerField

  数字属性

### 属性

- 外键

  ForeignKey

  - to

    字符串，数据表模型或者数据表模型的名称

  - on_delete

    被指向的对象被删除后当前数据表如何处理

  如果数据表的某个属性被指定为外键属性，则要求对象参数必须为外键指向的表的实例，需要使用get函数获得

  ```python
  @csrf_exempt
  def add(request):
      if request.method == 'GET':
          try:
              # 连接数据库
              city = request.GET['city']
              city = CityTable.objects.get(city=city) # get获得CityTable实例
              county = request.GET['county']
              CityCountyTable(city=city, county=county).save() # CityTable实例为city变量参数
              return HttpResponse(status=200)
          except ValueError as e:
              print(e)
              return HttpResponse(status=400)
      else:
          return HttpResponse(status=405)
  ```

- 双主键

  使用class Meta中的联合主键

## class Meta

定义数据表的行为

### 函数

- UniqueConstraint

  创建数据表中的限制——要求某些内容独特

  - 联合主键的创立

    ```python
    UniqueConstraint(fields=['room', 'date'], name='unique_booking')
    ```

## 数据库的增删改查

创建数据模型后，Django提供一套抽象API，允许增删改查

- 增

  一个模型类的实例代表一行记录

  使用model对象的save方法

  ```python
  from blog.models import Blog
  b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
  b.save()
  ```

  保存一个对象即为保存一条记录

- 删

  直接对QuerySet对象使用delete操作

- 改

  直接对QuerySet中的对象执行update操作

- 查

  使用模型类的manager（objects）来获取一个QuerySet

  一个QuerySet相当于一个select，一个filter相当于一个where

```python
AccountTable.objects.all()
```

返回包含整个数据表的QuerySet

```python
.filter(email=email)
```

数据库查询条件语句

```python
.values('password')
```

查询指定列

```python
AccountTable.objects.all().filter(email=email).values('password')
```

