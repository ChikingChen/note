# softmax

## 回归和分类

- 回归

  目标变量是连续性数值变量

- 分类

  目标变量是离散性数值变量

  结果为分类，过程为回归

### 线性回归

线性回归loss计算
$$
loss=y-\hat y
$$
总计表示
$$
Q=\sum^n_{i=1}(y_i-\hat y_i)^2=\sum^n_{i=1}(y_i-(\hat\beta_0+\hat\beta_1x_i)) ^2
$$

### 最小二乘法

计算Q什么时候取最小的
$$
\begin{align}
\frac{\partial Q}{\partial \beta_0}&=2\sum_{i=1}^n(y_i-\hat\beta_0-\hat\beta_1x_i)=0\\
\frac{\partial Q}{\partial \beta_1}&=2\sum_{i=1}^n(y_i-\hat\beta_0-\hat\beta_1x_i)x_i=0
\end{align}
$$
分别对$\beta_0$和$\beta_1$求导，取0得$\beta_0$和$\beta_1$结果

## softmax和sigmoid

- softmax：多个输出结点
- sigmoid：单个输出结点

$softmax$中的$loss$函数
$$
\begin{align}
loss_i&=-\log\frac{e^{z_i}}{\sum_{c=1}^Ce^{z_c}}\\
&=-z_i+\log\sum_{c=1}^Ce^{z_c}
\end{align}
$$
多分类问题中只有正确选项的$y_c$为$1$，代入得到$loss_i$的形式

