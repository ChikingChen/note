# [PCA](https://zhuanlan.zhihu.com/p/77151308)

## 矩阵变换

假设一个$n$维空间中向量要进行变换，我们可以使用$n*n$的矩阵进行，这个矩阵由$n$个$n$维行基向量组成。实际上如果我们增减这个基向量的数量，那么我们变换的结果向量维度也将变化。

我们令基行向量为$\mathbf t_{i}^T(1\leq i\leq n)$，假设我们要将$m$个向量$\mathbf s_i$转换为基向量空间中的向量，那么实际上的操作如下
$$
\begin{bmatrix}
\mathbf t_1^T\\
\mathbf t_2^T\\
\ldots\\
\mathbf t_n^T
\end{bmatrix}
\begin{bmatrix}
\mathbf s_1&\mathbf s_2&\ldots&\mathbf s_m
\end{bmatrix}
$$
通过计算我们能够得到$m$个向量在$n$个维度上的投影

首先看第一个维度
$$
\mathbf t_1^T\begin{bmatrix}\mathbf s_1&\mathbf s_2&\ldots&\mathbf s_m\end{bmatrix}=
\begin{bmatrix}
\mathbf t_1^T\mathbf s_1&\mathbf t_1^T\mathbf s_2&\ldots&\mathbf t_1^T\mathbf s_m
\end{bmatrix}
$$
我们观察$\mathbf t_1^T\mathbf s_i$实际上是$\mathbf s_i$在第一个维度上的投影，矩阵运算的结果则为$m$个向量在第一个维度上的投影

如此推测
$$
\begin{bmatrix}
\mathbf t_1^T\\
\mathbf t_2^T\\
\ldots\\
\mathbf t_n^T
\end{bmatrix}
\begin{bmatrix}
\mathbf s_1&\mathbf s_2&\ldots&\mathbf s_m
\end{bmatrix}
$$
如上的计算结果实际上是$m$个向量在$n$个维度上的投影

## 最大可分性

我们现在要确定的是，什么样的数据能够最大程度保留信息？一种直观的感受是投影后的投影值越分散越能保留信息。重叠将导致样本信息丢失。

### 方差

我们现在关注第一维的计算结果
$$
\mathbf t_1^T\begin{bmatrix}\mathbf s_1&\mathbf s_2&\ldots&\mathbf s_m\end{bmatrix}=
\begin{bmatrix}
\mathbf t_1^T\mathbf s_1&\mathbf t_1^T\mathbf s_2&\ldots&\mathbf t_1^T\mathbf s_m
\end{bmatrix}
$$
实际上是$m$个向量在$n$个维度上的投影，如果两个向量的投影相同，那么我们认为这两个向量在这个维度上特征相同，但是明显的是实际上这两个向量是两个无关的向量，因此我们我们希望这两个向量应当尽量不同，也就是应当加大方差

### 协方差

$$
\begin{bmatrix}\mathbf t_1^T\\\mathbf t_2^T\end{bmatrix}\begin{bmatrix}\mathbf s_1&\mathbf s_2&\ldots&\mathbf s_m\end{bmatrix}=
\begin{bmatrix}
\mathbf t_1^T\mathbf s_1&\mathbf t_1^T\mathbf s_2&\ldots&\mathbf t_1^T\mathbf s_m\\
\mathbf t_2^T\mathbf s_1&\mathbf t_2^T\mathbf s_2&\ldots&\mathbf t_2^T\mathbf s_m
\end{bmatrix}
$$

我们现在看两个映射向量的结果，$[\mathbf t_1^T\mathbf s_1,\mathbf t_1^T\mathbf s_2,\ldots,\mathbf t_1^T\mathbf s_m]$以及$[\mathbf t_2^T\mathbf s_1,\mathbf t_2^T\mathbf s_2,\ldots,\mathbf t_2^T\mathbf s_m]$，倘若两个向量存在线性相关性，那么这两个向量实际上能够相互表出，那么我们应当降低两者的线性相关性，也就是降低协方差为$0$

