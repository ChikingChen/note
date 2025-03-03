# [CS231n SVM](https://github.com/Halfish/cs231n)学习笔记

## SVM是什么

样本空间中使用一个超平面将所有样本划分开，在一个区域内的为一个类型，另一个区域内的为另一个类型

超平面表达式
$$
\begin{pmatrix}
x_1&x_2&\ldots&x_n
\end{pmatrix}
\begin{pmatrix}w_1\\w_2\\\ldots\\w_n\end{pmatrix}+b=0
$$

一个超平面将样本空间划分为两类，适合于01分类问题

## 问题1——svm_loss_naive

笔者在刚开始做这个东西时候确实啥也不懂，就拿份大佬的代码过来参考一下

> 大佬笔记出处[link](https://github.com/Halfish/cs231n)

```python
# Evaluate the naive implementation of the loss we provided for you:
from cs231n.classifiers.linear_svm import svm_loss_naive
import time

# generate a random SVM weight matrix of small numbers
W = np.random.randn(3073, 10) * 0.0001

loss, grad = svm_loss_naive(W, X_dev, y_dev, 0.000005)
print('loss: %f' % (loss, ))
```

函数代码

```python
def svm_loss_naive(W, X, y, reg):
  """
  Structured SVM loss function, naive implementation (with loops).

  Inputs have dimension D, there are C classes, and we operate on minibatches
  of N examples.

  Inputs:
  - W: A numpy array of shape (D, C) containing weights.
  - X: A numpy array of shape (N, D) containing a minibatch of data.
  - y: A numpy array of shape (N,) containing training labels; y[i] = c means
    that X[i] has label c, where 0 <= c < C.
  - reg: (float) regularization strength

  Returns a tuple of:
  - loss as single float
  - gradient with respect to weights W; an array of same shape as W 
  """
  dW = np.zeros(W.shape) # initialize the gradient as zero

  # compute the loss and the gradient
  num_classes = W.shape[1]
  num_train = X.shape[0]
  loss = 0.0
  for i in xrange(num_train):
    scores = X[i].dot(W)
    correct_class_score = scores[y[i]]
    for j in xrange(num_classes):
      if j == y[i]:
        continue
      margin = scores[j] - correct_class_score + 1 # note delta = 1
      if margin > 0:
        loss += margin
        dW[:, j] += X[i]
        dW[:, y[i]] -= X[i]

  # Right now the loss is a sum over all training examples, but we want it
  # to be an average instead so we divide by num_train.
  loss /= num_train
  dW /= num_train

  # Add regularization to the loss.
  loss += 0.5 * reg * np.sum(W * W)
  # print((W * W).shape)
  dW += reg * W

  #############################################################################
  # TODO:                                                                     #
  # Compute the gradient of the loss function and store it dW.                #
  # Rather that first computing the loss and then computing the derivative,   #
  # it may be simpler to compute the derivative at the same time that the     #
  # loss is being computed. As a result you may need to modify some of the    #
  # code above to compute the gradient.                                       #
  #############################################################################


  return loss, dW
```

### 本题意义大致理解

W是随机生成的一个权重矩阵，形状为(3073, 10)可以理解为(样本维度, 分类数量)，一个3073维的行向量能够唯一确定一个超平面，通过超平面打分我们能够衡量该样本在该超平面上的表现进而得到损失率。因为我们考虑到需要10个分类那么我们就需要10个超平面进行衡量，因此权重矩阵是10个3073维的列向量的堆叠。

我们需要计算的是该随机生成的权重矩阵的损失率以及梯度矩阵。

### 各种矩阵的形状和意义

X形状为(500, 3073)，500为样本数量，3073为每个样本特征数量

W形状为(3073, 10)，3073为单个图片通道数量，10为分类数量，可以理解为十个超平面列法向量的堆叠

XW形状为(500, 10)，500为样本数量，10为分类数量，其结果相当于每个样本在经过W权重矩阵打分后得到的每个分类的分数

### SVM loss计算

使用hinge loss算法
$$
L_i=\sum_{j\neq y_i}max(0, s_j-s_{y_i}+\Delta)
$$
$s_j$是编号为$j$（就是错误编号）的分数，$s_{y_i}$是正确编号的分数，我们可以认为我们希望错误编号分数越低而正确编号分数越高越好（预测结果直接选分数最高的就行了）

$\Delta$是我们给的一个偏置值，假设$s_j$小于$s_{y_i}$而偏置值为0，那么我们$max$之后的结果永远为$0$，我们当然希望$s_j$更小，因此加入一个正的偏置值能够度量这种小于

### SVM 梯度矩阵计算

> 本段参考[link](https://zhuanlan.zhihu.com/p/21478575)

$loss$是由$W$确定的，$L$可以理解为$loss$函数，写作$L(W)$

权重矩阵$W$可以看作是$10$个列向量组成一个矩阵，这种情况下， $L(W)$写作$L(w_0,\ldots,w_9)$，我们通过对这$10$个分量求梯度得到梯度矩阵
$$
\nabla L=(\frac{\partial}{\partial w_0}L, \frac{\partial}{\partial w_1}L,\ldots,\frac{\partial}{\partial w_9}L)
$$

- 如何计算$\frac{\partial}{\partial w_i}L$

  首先明确什么是$L$
  $$
  \begin{align}
  L_i&=max(0, s_j-s_{y_i}+\Delta)\\
  &=\sum_{j\neq y_i}[max(0, w_j^Tx_i-w_{y_i}^Tx_i+\Delta)]
  \end{align}
  $$
  

  求的方法大致理解为将$L$展开而后求导
  $$
  \begin{align}
  \frac{\partial}{\partial w_{y_i}}L&=-(\sum_{j\neq y_i}(w_j^Tx_i-w_{y_i}^T+\Delta)>0)x_i=-\lambda_1x_i\\
  \frac{\partial}{\partial w_j}L&=(w_j^Tx_i-w_{y_i}x_i+\Delta>0)x_i=\lambda_2x_i
  \end{align}
  $$
  该结果表示，在不考虑常数的情况下我们应当对错误结果减去$x_i$，对正确结果加上$x_i$，也就是在第$j$列减去$x_i$并对第$y_i$列加上$x_i$


### 代码解释

- ```python
  dW = np.zeros(W.shape) # initialize the gradient as zero
  ```

  梯度矩阵初始化

- ```python
  scores = X[i].dot(W)
  correct_class_score = scores[y[i]]
  ```

  i号样本与权重矩阵相乘，并提取正确的分数

- ```python
  for j in xrange(num_classes):
    if j == y[i]:
      continue
    margin = scores[j] - correct_class_score + 1 # note delta = 1
    if margin > 0:
      loss += margin
  ```

  hinge算法计算loss总和

- ```python
  dW[:, j] += X[i]
  dW[:, y[i]] -= X[i]
  ```
  根据hinge算法计算梯度矩阵

- ```python
  loss += 0.5 * reg * np.sum(W * W)
  dW += reg * W
  ```

  $L2$正则化，涉及过拟合的问题，非必需项，不作详细讨论



