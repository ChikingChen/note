# XY-分层

XY分层用来处理递归sql中聚合更新等非单调操作

## 核心思想

- X层：迭代层，外层循环的迭代次数
- Y层：阶段层，X内部把非单调运算的更新按“安全的顺序”进行

同一层X中，将一个循环分割成多个阶段，每个阶段只做单调操作，非单调操作只放到相邻阶段之间

## group by关键字

```mysql
SELECT product, SUM(amount) AS total_amount
FROM sales
GROUP BY product;
```

group by为指定聚合条目的聚合函数，实际的聚合规则在语句中隐性定义

## 窗口函数

```mysql
<函数名>([表达式]) 
OVER (
    [PARTITION BY 列1, 列2, ...]  -- 按组划分窗口
    [ORDER BY 列3, ...]           -- 窗口内的排序规则
    [ROWS 或 RANGE 窗口帧定义]     -- 限定从哪行到哪行
)
```

over的作用和group by类似，但over不会将所有行压缩成结果而是将聚合的结果作为一个新的属性出现在结果中

## 从非单调递归到单调递归

```mysql
WITH RECURSIVE dist (node, d) AS (
    -- 基例：起点 A
    SELECT 'A', 0
    UNION ALL
    -- 递归步：传播并直接更新（MIN）
    SELECT e.dst, LEAST(d.d + e.w, IFNULL(dist2.d, 1e9))
    FROM dist d
    JOIN edges e ON e.src = d.node
    LEFT JOIN dist dist2 ON e.dst = dist2.node
)
SELECT node, MIN(d) AS shortest
FROM dist
GROUP BY node;
```

上面这个是一个非单调递归，他将min聚类放在递归内部，这实际上会造成非单调性的问题，因为可以认为这个算法会将所有的**当下最优解**都存入dist数据表中，并且作为下一轮迭代的依据。实际上如果我们在这个阶段结束之后对每个节点进行一次聚类就行了。

