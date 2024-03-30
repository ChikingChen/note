# python连接数据库

使用pymysql连接mysql

```python
db = pymysql.connect(
    host='127.0.0.1', # 数据库
    user='root', # 用户名
    password='123456', # 密码
    database='db' # 数据库名称
)
```

pymysql.connect连接数据库，db为数据表连接

```python
cursor = db.cursor()
```

数据库游标，指向数据库中的内容

```python
sql = "insert into accounttable value ('15726922163', 'chiking', '123456', '2024-3-30', 0, 0, '2002-7-18');"
cursor.execute(sql)
```

使用cursor执行sql语句

- 修改数据表的sql语句

  ```python
  db.commit()
  ```

  使用db.commit()提交修改

- 从数据表中获取信息

  ```python
  cursor.fetchall()
  ```

  使用cursor.fetchall()获取信息

```python
db.close()
```

连接断开