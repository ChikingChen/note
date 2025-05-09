# sift

## sift的意义

提取出对旋转、尺度缩放以及亮度比那花保持不变并对视角变化、仿射变化以及噪声保持一定程度的稳定性

## 特征

### 颜色特征

- 颜色直方图

  不同色彩在整幅图像中所占的比例

### 纹理特征

- 灰度共生矩阵

  ![这里写图片描述](https://i-blog.csdnimg.cn/blog_migrate/483e577b663f87a045797b337a25b21e.gif)

  意义：从灰度为i的像素点出发，离开固定位置的点上灰度为j的概率

  特点：纹理变化缓慢的图像，灰度共生矩阵对角线数值较大；纹理变化较块的图像灰度共生矩阵对角线两侧数值较大

  有n个灰度等级，那么灰度共生矩阵的形状就是$n\times n$，第i行第j列的元素则代表距离为d的灰度i和灰度j的数量

  - 对比

    灰度直方图：灰度数量的一阶统计

    灰度共生矩阵：灰度特征的二阶统计

### 方向梯度直方图

- 图像预处理

  两种方向分别为是否使用灰度处理，使用灰度处理的图像不需要额外的梯度计算，未使用灰度处理的图像在后面的梯度计算时，对一个特定的像素格选择三通道中梯度较大的值

  - 伽马矫正

    减少光照对于图像的影响
    $$
    f(x)=x^\gamma
    $$
    取$\gamma=1.5$的情况下，使用如下python代码进行伽马矫正

    ```python
    img2 = np.power(img/float(np.max(img)), 1.5)
    ```

    大致过程如下（假设使用灰度处理）：

    先将读入的图片处理为灰度图像，并将灰度图像中每一个像素除以灰度图像中像素最大值，将所有的灰度值映射到$[0,1]$中显示，最后将所有的灰度值乘以$1.5$次方得到最后答案

    乘以$1.5$次方能够增强图像的对比度，加强效果

- 计算梯度图

  使用内核为$1$的$Sobel$算子计算梯度图

  ![img](https://pic3.zhimg.com/v2-13f7d8dc5cf43bd204b9f6fc93755bf8_1440w.jpg)

  使用sobel算子即可计算x方向梯度图以及y方向梯度图

  根据梯度的合梯度幅值和方向的计算方式
  $$
  \begin{align}
  g=\sqrt{g_x^2+g_y^2}\\
  \theta=\arctan\frac{g_y}{g_x}
  \end{align}
  $$
  即可计算出关于原图的幅值和梯度方向图

- 计算梯度直方图

  将整个图像划分为$8\times8$的小单元，计算小单元的梯度直方图。梯度图计算过程中我们获得了梯度幅值图以及梯度方向图。我们使用一个直方图来统计这两者之间的关系。

  直方图被分为9份，每一份用于统计$(i-1)*20$度数的幅值（$0,20,\ldots,160$等等），倘若出现37这样的数值，则将按比例分配给0和20。

### Block归一化

将$2\times2$个小单元归纳成一个块，$2\times2$个小单元则有$36$个值，对这36个值进行归一化即可得到36个特征值

![动图](https://picx.zhimg.com/v2-2324177420c0a79d48d844b5e124f1f3_b.webp)

使用滑动窗口形式获取所有的块的特征值首尾相接得到整个图片的$HOG$特征向量

> 一个块的直方图表示了这个块内的边缘特征，HOG直方图则表示了所有块内的边缘特征
>
> 块和滑动窗口的概念模拟了人眼的识别过程

## sift算法步骤

### 尺度空间极值检测

- 构建高斯金字塔

  - 图像逐层缩小（即下采样），每层图像即为一个组
  - 每个组内次啊用多个不同标准差的高斯模糊生成多个尺度空间（一个标准差能够产生一个尺度空间），形成高斯金字塔

- 计算高斯差分金字塔

  - 对每个组内相邻尺度的高斯模糊图像进行相减，得到DOG图像，即为DOG金字塔

- 检测极值点

  - 对于DOG金字塔中图像的每个像素点检测当前尺度图片以及相邻尺度图片的26个邻居（8个相邻点以及相邻尺度对应的两个像素点以及对应像素点的8个相邻点），如果当前点为极值，则将其作为一个候选关键点

  > - 什么样的点会被作为极值点选入？
  >
  >   高斯金字塔中同一层的图片从左到右可以看作是高斯模糊不断加强的过程，我们可以想，不断模糊过程中非特征点的消失是连续的。假设自己是一个不断加剧的近视眼，10秒钟时间内眼镜度数从0涨到了1000，这种情况下，假设你眼前有一个模型，模型的连续色块的变化实际上较少，而模型的特征点——例如角还有边等等——变化则在近视眼加剧过程中的某个时刻将突然消失，这个突然消失的点则是我们的极值点也就是特征点。
  >
  > - 为什么要模拟远近程度？
  >
  >   考虑到我们是要完成对多张图片上相同物体的检测以及匹配，这就涉及到对相同物体拍摄的角度，远近，光线等等因素的匹配，本质上来说其要完成在不同情况下物体的匹配。在远近程度的模拟上我们选择了使用高斯金字塔的形式来完成。而高斯差分金字塔的形式则是为了提取物体中的特征点。

- 精确定位——极值点的精确

- 确定关键点方向

  通过分析关键点领域梯度分布来分配主方向

  领域范围：关键点为中心，半径为$1.5\sigma$的圆形邻域

  使用sobel算子计算关于x、y的导数，进而计算梯度方向以及梯度幅值，在这个圆形邻域内构建方向直方图

  将直方图中的峰值方向作为主方向，次峰值方向作为辅助方向

  > 可以使用高斯滤波来对图像进行处理，防止方向突变
  - 旋转不变性的实现

    旋转之后图像的关键点方向旋转相同角度，在对比中可以通过角度矫正达到旋转不变性的效果

  > 关键点方向的计算实际上是旋转不变性的保证
  >
  > 关键点方向实质上是该关键点领域范围内占主导地位的梯度方向，通过同一关键点方向能够保证旋转不变性的实现

- 构建关键点描述符

  现有关键点以及方向，需要构建关键点描述符

  - 邻域范围选择

    选择大小为$16\sigma\times16\sigma$的邻域

  - 梯度计算

    使用$Sobel$算子计算关于$x,y$方向的导数进而计算梯度幅值以及梯度方向

  - 构建梯度方向直方图，直方图主峰即为关键点的方向

  - 旋转对齐

    将领域坐标轴旋转至主方向，保持后续描述符的旋转不变性

  - 领域分块

    旋转后的领域划分为$4\times4$的子区域

  - 子区域梯度直方图

    子区域梯度直方图划分为8个方向（每个方向分为$45°$）统计梯度直方图，得到一个8维的特征向量，将$4\times4=16$个子区域的$8$为特征向量合并为一个$128$维的特征向量，即为当前特征点的特征描述符

  > - 如何理解旋转对齐？
  >
  >   假设两张照片中存在两个本质上相同的关键点，但是因为拍摄角度的问题导致两者关键点周围的邻域中的像素梯度实质上存在方向上的差异。但因为图像本质上的相同又导致邻域中的梯度方向以及梯度幅度在旋转后实质上相同。因此我们将邻域中的主方向归零作为同一标准，实质上实现了旋转不变性。
  >
  > - 如何理解使用梯度直方图归一化向量作为sift的最后结果？
  >
  >   将旋转后的区域划分为$16$块，每块统计梯度直方图，实质上将关键点周围的内容简化为梯度并统计——即使用16个8维向量代表关键点周围的信息。

## sift数学原理

### 高斯模糊

使用$\sigma$不同的高斯卷积核对图片进行高斯模糊，形成一系列不同标准差的高斯模糊，进而构建尺度空间

### 高斯差分

减去两个不同标准差的高斯模糊图像得到，通过尺度空间的差分，能够得到一系列差分图像

### 精确定位（亚像素级精确定位）

高斯差分函数$D(x,y,\sigma)$二阶泰勒级数展开
$$
D(x)\approx D+\nabla D^T(x-x_0)+\frac{1}{2}(x-x_0)^TH(x-x_0)
$$

- $x$为位置向量，$x_0$为极值点位置
- $D$为候选关键点的$DoG$值
- $\nabla D$为候选关键点的梯度向量
- $H$为候选关键点的$Hessian$矩阵

> #### 为什么不能直接求导而是要在二阶泰勒级数展开之后再求导
>
> 如果直接求导结果为$\frac{\part D}{\part x}=\nabla D$。我们先要了解的是这边的$D$描述了高斯差分金字塔同一层中在确定标准差以及像素位置的情况下，像素的值。我们现在对这样一个函数求导得到了关于这个函数的梯度，可以发现的是我们通过卷积等等方式我们能够得到当前点的梯度而后我们通过寻找趋于0的梯度的方式能够找到若干的点。但这样的结果只能做到像素级，而不是亚像素级，精确度有限。

对二阶泰勒展开求导得到
$$
\begin{align}
\frac{\part D}{\part x}=\nabla D^T+H\Delta x^T&=0\\
\Rightarrow\nabla D+\Delta xH&=0\\
\Delta x&=-H^{-1}\nabla D
\end{align}
$$
对于偏移值大于0.5的点删除——偏向别的像素点和极值点的意义相悖

关键点对比度（即被投射到[0,1]区间内的亮度）小于某个阈值删除——减少较暗的边缘以及噪声的影响

## 总结

1. 高斯金字塔

   通过高斯模糊在同一尺度空间中寻找特征

   通过下采样模拟远近过程

2. 高斯差分金字塔

   对比同一尺度空间前后图像，得到极值点

3. 精确定位

   使用多元函数泰勒展开对极值点做出评价，舍去不合适的极值点

4. 将修正后的坐标定位回高斯金字塔

   将高层次的关键点定位到低层次

5. 修正主方向并计算关键点领域的$4\times 4$梯度直方图

   使用关键点领域的梯度直方图向量来作为关键点的向量

# surf

## 积分图

使用二维前缀和快速计算限定区域内所有像素的和

## $Hessian$矩阵近似

$Hessian$矩阵
$$
H(x,y,\sigma)=
\begin{pmatrix}
L_{xx}&L_{xy}\\
L_{xy}&L_{yy}
\end{pmatrix}
$$
通过卷积计算$L_{xx},L_{xy},L_{yy}$
$$
det(H)=L_{xx}L_{yy}-L_{xy}^2
$$
