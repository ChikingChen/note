# GAS

GAS将图计算的核心步骤拆分为三个阶段：收集，应用，传播

- 收集：从当前顶点的邻居收集信息

- 应用：通过收集到的信息更新自己节点的状态
- 传播：将更新后的结果发送给所有邻居

## GAS模型的SSSP算法

```python
Input:
  G = (V, E)              // 图
  w(u, v) >= 0            // 边权
  s                       // 源点
Initialize:
  for v in V:
    dist[v] := +∞
  dist[s] := 0

repeat until no vertex updates (converged) or iter >= MaxIters:

  # Gather：从入邻居收集候选距离
  for v in V in parallel:
    cand[v] := min_{(u -> v) in E} ( dist[u] + w(u, v) ) 
               # 若 v 没有入边，则 cand[v] := +∞

  # Apply：应用更新（仅当更优）
  active := false
  for v in V in parallel:
    if cand[v] < dist[v]:
       dist[v] := cand[v]
       flag[v] := true     # 本轮被更新
       active := true
    else:
       flag[v] := false

  # Scatter：把更新后的距离“传播”给出邻居（下一轮可见）
  # （在许多系统里，Scatter 只是决定谁在下一轮参与计算/发消息）
  frontier := { v | flag[v] == true }

until active == false

Output: dist[*]

```

每个点通过邻居获取来更新自己的状态（就是最近点）如此循环往复