# SQL-G_Efficient_Graph_Analytics_by_SQL

- 研究问题

  1. 图和关系数据的管理

     图数据存储和处理过程中的差异问题，图数据和非图关系数据一起存储，但是处理却是作为图结构进行处理，这两者之间存在较大差别，造成了开销

  2. 图分析和数据分析的统一处理

     图分析结构和普通关系型数据的分析结构的不同

  可以说，图数据管理系统和普通的关系型数据两者在本质上相似，但是因为结构和处理方式的不同造成了额外开销，我们现在的问题主要聚焦于这两者应当如何形成一种统一，能够同时兼顾编写效率以及分析效率两者

- mysql中的with

  mysql中通过with语句实现递归查询

  允许开发人员创建一个内存中的临时结果集，允许在后续的查询中多次使用

  ```sql
  WITH cte_name (column_name1, column_name2, ...) AS ( SELECT column1, column2, ... FROM table WHERE condition )
  ```

  - cte_name

    with子句的名称

  - column_name1, column_name2, ...

    结果集的列名

  with as可以认为是一个从句，通过with as语句创建出一个名为cte_name的临时表，通过cte_name的临时表来做进一步的操作，前提是都要有with as的语句作为从句

  - with recursive ... as ...

    ```sql
    WITH RECURSIVE cte (n) AS  
    (
      SELECT 1  #初始值
      UNION ALL
      SELECT n + 1 FROM cte WHERE n < 5  #递归内容 from cte 表
    )
    SELECT * FROM cte;
    ```

    - SELECT 1 

      在每个查询开始的时候都会加入1，考虑到第一次查询时候cte为空表，同时后面查询时候union自动去重，因此1成为了初始值

    - UNION ALL

      将两个select合并成为一个表

    - SELECT n + 1 FROM cte WHERE n < 5

      将cte中的所有n提取出来并且加一然后合并为一个新的表

    - SELECT * FROM cte;

      前面with从句生成了cte表，即为我们所要求的表，那么我们通过SELECT * FROM cte;来获得cte表中所有的数据

- mysql中迭代查询的不足之处

  mysql中的迭代查询只能用于查询，而无法做到结合具体的算法，例如最短路算法等等，其功能往往有限

- 图分析算法中的“半环”

  - “群”

    在集合的基础上，引入二元运算的概念，要求二元运算具有**结合律**、**单位元**和**逆元素**

  - “环”

    一种有两种运算的群，记为+和×

    - 对于+

      结合性，交换性，幺元，**逆元**

    - 对于×

      结合性，幺元

    - ×对＋具有分配性

  - “半环”

    相对于“环”的概念去掉+逆元的概念，也就是在“半环”中没有逆元的概念

  - 图分析中的半环

    图分析和半环的联系主要集中于半环的＋和×符号到图分析中的操作中的映射

    - +：路径聚合，合并多个路径的结果，例如最短路算法中的取min操作
    - ×：路径连接，连接多个路径产生新的路径，例如最短路算法中的“加法聚合”

    半环操作造成了已经查询的边信息的更新，导致了sql中的迭代查询失败

- sql中的半环操作

  

