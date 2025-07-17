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

我们现在看两个映射向量的结果，$[\mathbf t_1^T\mathbf s_1,\mathbf t_1^T\mathbf s_2,\ldots,\mathbf t_1^T\mathbf s_m]$以及$[\mathbf t_2^T\mathbf s_1,\mathbf t_2^T\mathbf s_2,\ldots,\mathbf t_2^T\mathbf s_m]$，倘若两个向量存在线性相关性，那么这两个向量实际上能够相互表出，那么我们应当降低两者的线性相关性，也就是降低协方差为$0$，同时我们兼顾之前在方差中说的，一个向量其方差应该尽量大，因此我们能够提出我们对第二个向量的期望：与第一个向量之前的协方差$0$，向量各元素之前方差应该尽量大

### 协方差矩阵

#### 方差、协方差计算过程中的均值归0

我们观察方差和协方差的计算公式
$$
\begin{align}
DX&=\frac{1}{n}\sum_{i=1}^n(X_i-\mu_X)^2\\
Cov(x,y)&=\frac{1}{n}\sum_{i=1}^n(X_i-\mu_X)(Y_i-\mu_Y)
\end{align}
$$
我们发现这里均值比较麻烦，为了简化计算我们将均值化为$0$，也就是$\mu_X=0,\mu_Y=0$，那么这种情况下，方差和协方差实际上变为
$$
\begin{align}
DX&=\frac{1}{n}\sum_{i=1}^nX_i^2\\
Cov(x,y)&=\frac{1}{n}\sum_{i=1}^nX_iY_i
\end{align}
$$
实际上，这种情况下能够大大降低我们的计算复杂度

#### 协方差矩阵

我们将所有的目标向量堆叠成一个矩阵
$$
X=\begin{bmatrix}
a_1&b_1\\
a_2&b_2\\
\vdots&\vdots\\
a_n&b_n
\end{bmatrix}
$$
将其与本身的转置相乘并除以$m$即可得到
$$
C=\frac{1}{m}X^TX=
\begin{bmatrix}
\frac{1}{m}\sum_{i=1}^ma_i^2&\frac{1}{m}\sum_{i=1}^ma_ib_i\\
\frac{1}{m}\sum_{i=1}^ma_ib_i&\frac{1}{m}\sum_{i=1}^mb_i^2
\end{bmatrix}
=\begin{bmatrix}
Cov(a,a)&Cov(a,b)\\
Cov(b,a)&Cov(b,b)
\end{bmatrix}
$$
我们观察这个矩阵$C$发现矩阵的对角线为向量内部的方差，其余为向量之间的协方差，我们应当能够找到一个矩阵$X$使得方差应当最大化，协方差应当为$0$，即特征矩阵

我们现在假设存在一个系数矩阵即$P$，使得$Y=PX$，在经过系数矩阵的变换之后$D=\frac{1}{m}YY^T$实际上就是特征矩阵，经过计算我们得到矩阵$C$和矩阵$D$之间的关系如下
$$
D=P^TCP
$$
现在我们的目标即转化为找到一个矩阵$P$使得矩阵$P^TCP$能够成为一个对角矩阵

实际上关于实对称矩阵，必然存在一个矩阵$E$能够使得$E^TCE$是对称矩阵，具体的过程为考研数学基本知识点，而我们要找的$P$实际上等于$E$，我们观察$P$的形状为$n*n$我们能够将其视为$n$个行向量的堆叠，可以理解的是经过$n$维向量$P$的变换$D$这个协方差矩阵中的方差都已经最大化，协方差都已经为$0$，那么这种情况下，我们只取$P$的上面$k$行那么计算出来的协方差矩阵也能有如此效果，因此假设我们要将$n$维向量降维到$m$维，那么我们就只需要取$D$矩阵特征值从大到小排列对应的$P$矩阵的前$m$行即可

- 为什么这里的样本能够直接求协方差？

  实际上样本本身就代表着和单位矩阵相乘，也就是说其本身即为一种变换，现在我们看这个变换的结果，并且计算其协方差矩阵对其进行评价。当然这种变换的方式实际上非常不严谨，因此我们要在单位矩阵的基础上再乘一个系数矩阵$P$，对其协方差矩阵做出调整，因此我们将目标集中于协方差矩阵之上，希望能够求出一个系数矩阵$P$，使得协方差矩阵能够满足我们的期待

- 如何理解减去均值？

  测试集中的数据通过减去均值，来降低运算难度。这使得$C$通过矩阵和矩阵转置的乘法，能够计算得到协方差矩阵

