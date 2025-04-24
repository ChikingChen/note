# python 八股

- 解释性语言：做好的程序，翻译一句，执行一句

  翻译性语言：做好的源程序，全部编译成**二进制**可运行程序

- 字符串：用引号括起来的任意文本

  列表：有序集合，可以增删

  元组：有序集合，不可以增删

  字典：无序集合，类似于C++中的map

  集合：类似C++中的set

- 字符串常用方法

  切片：[1:3]

  format："welcome to luobodazahui, dear {name}"format(name="baby") 在字符串中用大括号括住一个变量，然后再format中填充

  join：s = ' '.join(('a', 'b', 'c')) 第一个字符是分隔符，后面的是元组

  .replace：'ababab'.replace('ab', 'ccc', 1)  第一个字符串是被替换字符串，replace中分别为被替换对象，替换的内容以及替换次数

  .split：'luobo,dazahui good'.mystr5.split('h') 第一个字符串是被分割字符串，split中的对象是分割的边界

- append extend

  append：mylist1.append(mylist2)将mylist2作为成员加入mylist1

  extend：mylist1.extend(mylist2) 遍历mylist2中的成员并全部append进入mylist1

- del pop remove

  del mylist4[0] 删除mylist4中0号位置上的成员

  mylist4.pop() 删除mylist4最后一位的数字

  mylist4.remove('c') 删除mylist4中一个'c'成员

- sort reverse

  mylist.sort() 对mylist从小到大排序

  mylist5.reverse() 对mylist反转

- 字典清空

  dict.clear() 清空 dict 这个字典

- 指定删除

  dict1.pop('key1') 指定删除key1这个键值

- 三种遍历字典的方式

  mykey = [key for key in dict2]：for key in dict2 返回字典中所有键值

  myvalue = [value for value in dict2.values()]：for value in dict2.**values()** 返回字典中所有键值对应的值

  key_value = [(k, v) for k, v in dict2.items()]：for k, v in dict2.**items()** 返回字典中所有键值以及键值对应的值

- dict.fromkeys(keys, 0)

  keys为一个列表，0为键值，列表中所有值都对应一个固定的键值，构成一个字典

- unicode utf-8

  unicode 所有语言字符编码**标准**

  utf-8 可变长编码方式，使用1 - 4个字节来表示一个字符，进而表示unicode中的所有字符，实现这个这**标准**的一种方式

- encode decode

  "你好".encode('utf-8') 字符串编码为utf-8

  b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8') 字符串转码为源码

- 一行代码数值交换

  a, b = b, a

- ==   is

  ==：数值相等

  is：地址相等

- python 函数参数类型

  - 必选参数

    ```python
    def test_divide(num1, num2):
        return num1/num2
    ```

    必须填写的参数

  - 可选参数（默认参数）

    ```python
    def test_add(num=1):
      return num + 1
    ```

    可以不填写的参数，不填写num即为1

  - 位置参数

    ```python
    def test_divide(num1, num2, /):
      return num1/num2
    
    print(test_divide(1,2))
    print(test_divide(num1=1,num2=2))
    ```

    / 之前的参数不能使用变量名来引用

  - 可变参数

    ```python
    def test_sum(*numbers):
        print(numbers)
        print(type(numbers))
        sum = 0
        for num in numbers:
            sum = sum + num
        return sum
    
    print(test_sum(1,2,3,4))
    ```

    将多个参数装进元组，使用*arg指明

  - 关键字参数

    ```python
    def person(**message):
        print(message)
        print(type(message))
        if 'name' in message:
            print(f"my name is {message['name']}")
    
    person(name='zhangsan', age=18)
    ```

    将多个参数装进字典（使用关键字指明对应关系），只用**kwarg指明

- 当前时间

  ```python
  datetime.datetime.now()
  ```

  使用datetime库来获取时间

- PEP8 基本规范  https://blog.csdn.net/fgf00/article/details/107890522

  - 使用4个空格进行缩进
  - 在, : #之后加一个空格
  - 在+ - * /两边各加一个空格
  - 使用\ ()控制换行

- 

