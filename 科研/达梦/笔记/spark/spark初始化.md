# spark初始化

在spark入门书籍中，提供了一份基础spark代码，但是这份代码在jupyter中无法正常运行

```python
from pyspark import SparkConf, SparkContext
conf = SparkConf().setMaster("local").setAppName("My App")
sc = SparkContext(conf = conf)
```

原因是pyspark启动过程中将会自动创建一个SparkContext对象，如果我们后面再创建一个SparkContext对象，那么就会产生重复的问题，一个程序中只能存在一个SparkContext，这个存在的SparkContext对象命名为sc

