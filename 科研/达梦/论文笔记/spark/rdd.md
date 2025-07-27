# RDD

理解为spark中的普通对象，与普通对象的区别在于能够在spark中参与分布式运算

## 创建RDD的两种方式

- 读取已有的内容

  ```python
  lines = sc.textFile("/path/to/README.md")
  ```

- 使用parallelize api创建

  ```python
  lines = sc.parallelize(["pandas", "i like pandas"])
  ```

## RDD操作

- 转化操作

  ```python
  inputRDD = sc.textFile("log.txt")
  errorsRDD = inputRDD.filter(lambda x: "error" in x)
  ```

  生成一个新的RDD操作

- 行动操作

  ```python
  print("Input had " + badLinesRDD.count() + " concerning lines")
  print("Here are 10 examples:")
  for line in badLinesRDD.take(10):
  	print(line)
  ```

  对已有的RDD进行计算

只有进行行动操作时候RDD的操作才会真的计算，这被称为**惰性求值**

## 具体RDD操作

关注《Spark快速大数据分析》3.5

- rdd.reduce(func)

  func必须是一个二元函数，同时保证二元函数的类型一定相同

  ```python
  # 创建 RDD
  numbers = sc.parallelize([1, 2, 3, 4, 5])
  
  # 使用 reduce 求和
  total = numbers.reduce(lambda a, b: a + b)
  print("总和:", total)  # 输出: 15
  ```

- rdd.fold(zeroValue)(func)

  与reduce类似，需要设置初始值

  ```python
  # 创建 RDD（两个分区）
  numbers = sc.parallelize([1, 2, 3, 4], 2)
  
  # 使用 fold 求和，初始值为 0
  total = numbers.fold(0)(lambda a, b: a + b)
  print("总和:", total)  # 输出: 10
  ```

  在parallelize过程中设定分区数量，rdd在计算过程中将会自动并行化

  上述过程中将数据分为了两块，那么在计算过程中，两块将会分别加和后将两块的结果再加和得到最终的结果

  reduce和fold要求输入和输入结果类型必须相同

- rdd.aggregate(zeroValue, seqOp, combOp)

  ```python
numbers = sc.parallelize([1, 2, 3, 4, 5])
  zero_value = (0, 0)
seq_op = lambda acc, num: (acc[0] + num, acc[1] + 1)
  comb_op = lambda acc1, acc2: (acc1[0] + acc2[0], acc1[1] + acc2[1])
  sum_count = numbers.aggregate(zero_value, seq_op, comb_op)
  average = sum_count[0] / sum_count[1]
  print("平均值:", average)
  ```
  
  能够返回与rdd类型不同的值
  
  - zeroValue：初始值
  - seqOp：将元素合并到累加器
  - combOp：将多个累加器合并
  
  seqOp以及combOp都能够通过lambda函数来定义
  
  
  
  