# [PCA](https://blog.csdn.net/qq_52358603/article/details/146534765)

我们假设存在这样一种列向量
$$
\mathbf A=\begin{bmatrix}
x_A\\
y_A\\
x_B\\
y_B\\
x_C\\
y_C\\
\end{bmatrix}
$$
它默认了一组线性基如下
$$
\mathbf B=\begin{bmatrix}
\mathbf b_1\\
\mathbf b_2\\
\mathbf b_3\\
\mathbf b_4\\
\mathbf b_5\\
\mathbf b_6\\
\end{bmatrix}=
\begin{bmatrix}
1&0&0&0&0&0\\
0&1&0&0&0&0\\
0&0&1&0&0&0\\
0&0&0&1&0&0\\
0&0&0&0&1&0\\
0&0&0&0&0&1\\
\end{bmatrix}=I
$$

## 数据的干扰因素

一者是噪声，一者是冗余，如果两个独立测量量出现了正相关性，那么就认为这两个独立测量量有冗余的问题

- 为什么方差要大？

  方差大说明数据之间的差异大，因为本质上来说一个向量的多个数据之间本质上没有差别，但是如果噪声较大的情况下将导致差异被抹平，这种情况下方差变小

- 为什么协方差要小？

  协方差大的情况下说明不同向量之间存在较强的线性相关性，也就是冗余

