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

## QuerySet

