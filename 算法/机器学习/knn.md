# 作业1

## knn

knn的分类器，将过程划分为两个步骤

1. 计算所有测试样例和训练样例之间的距离
2. 通过前k个最接近的样例得到答案

可以认为每个图像的像素范围相同（0到255），数量相同，可以归一化

归一化结果偏小显示为黑，偏大显示为白

Q1：每一行为指定测试数据和所有训练数据的距离，距离偏大则为白色

Q2：每一列为指定训练数据和所有测试数据的距离

## 问题2

两种平均值 1. 所有图片所有像素的平均值 2. 所有图像同一位置像素的平均值

两种对应的标准差

1. 不变
2. 改变
3. 不变
4. 改变
5. 不变

## 单循环

```python
dists[i][j] = np.sqrt(np.sum((X[i] - self.X_train[j]) ** 2))
```

两个向量相减根据勾股定理得到向量的距离

- X[i] - self.X_train[j]

  向量相减

- (X[i] - self.X_train[j]) ** 2

  相减的向量平方

- np.sum((X[i] - self.X_train[j]) ** 2)

  使用np.sum求和

- np.sqrt(np.sum((X[i] - self.X_train[j]) ** 2))

  使用np.sqrt对求和结果开方

```python
dists += np.sum(self.X_train ** 2, axis=1).reshape(1, num_train)
dists += np.sum(X ** 2, axis=1).reshape(num_test, 1) # reshape for broadcasting
dists -= 2 * np.dot(X, self.X_train.T)
dists = np.sqrt(dists)
```

$$
\begin{align}
dis[i][j] &= \sqrt{\sum_{k=0}^n (X[i][k]-self.X\_train[j][k]) ^ 2}\\
&=\sqrt{\sum_{k=0}^nX[i][k]^2+\sum_{k=0}^nself.X\_train[j][k] ^ 2 - 2\sum_{k=0}^nX[i][k]\times self.X\_train[j][k]}
\end{align}
$$

- $dists$，$self.X\_train$和$X$形状

  $self.X\_train$训练数据，shape——(训练数据数量， 训练数据维度)

  $X$测试数据，shape——（测试数据数量，测试数据维度）

  $dists$第i个测试数据和第j个训练数据之间的距离，shape——（测试数据数量，训练数据数量）

- ```python
  dists += np.sum(self.X_train ** 2, axis=1).reshape(1, num_train)
  dists += np.sum(X ** 2, axis=1).reshape(num_test, 1) # reshape for broadcasting
  ```

  将训练数据按行乘方加和得到行向量，测试数据得到列向量

- ```python
  dists -= 2 * np.dot(X, self.X_train.T)
  ```

  X形状——（测试数据数量，数据维度）

  self.X_train形状——（训练数据数量，数据维度）

  结果形状——（测试数据数量，训练数据数量）

- ```python
  dists = np.sqrt(dists)
  ```

  对结果开方

```python
num_folds = 5
k_choices = [1, 3, 5, 8, 10, 12, 15, 20, 50, 100]

X_train_folds = []
y_train_folds = []
avg_size = int(X_train.shape[0] / num_folds) 
for i in range(num_folds):
    X_train_folds.append(X_train[i * avg_size : (i+1) * avg_size])
    y_train_folds.append(y_train[i * avg_size : (i+1) * avg_size])
    
k_to_accuracies = {}
for k in k_choices:
    accuracies = []
    print k
    for i in range(num_folds):
        X_train_cv = np.vstack(X_train_folds[0:i] + X_train_folds[i+1:])
        y_train_cv = np.hstack(y_train_folds[0:i] + y_train_folds[i+1:])
        X_valid_cv = X_train_folds[i]
        y_valid_cv = y_train_folds[i]
        
        classifier.train(X_train_cv, y_train_cv)
        dists = classifier.compute_distances_no_loops(X_valid_cv)
        accuracy = float(np.sum(classifier.predict_labels(dists, k) == y_valid_cv)) / y_valid_cv.shape[0]
        accuracies.append(accuracy)
    k_to_accuracies[k] = accuracies
    
for k in sorted(k_to_accuracies):
    for accuracy in k_to_accuracies[k]:
        print 'k = %d, accuracy = %f' % (k, accuracy)
```

- ```python
  X_train_folds = []
  y_train_folds = []
  avg_size = int(X_train.shape[0] / num_folds) 
  for i in range(num_folds):
      X_train_folds.append(X_train[i * avg_size : (i+1) * avg_size])
      y_train_folds.append(y_train[i * avg_size : (i+1) * avg_size])
  ```

  将训练数据分段

- ```python
  for i in range(num_folds):
  ```

  枚举用来交叉验证的块

- ```python
  X_train_cv = np.vstack(X_train_folds[0:i] + X_train_folds[i+1:])
  y_train_cv = np.hstack(y_train_folds[0:i] + y_train_folds[i+1:])
  ```

  将训练特征竖直堆叠，将训练标签水平堆叠

- ```python
  X_valid_cv = X_train_folds[i]
  y_valid_cv = y_train_folds[i]
  ```

  验证数据

- ```python
  accuracy = float(np.sum(classifier.predict_labels(dists, k) == y_valid_cv)) / y_valid_cv.shape[0]
  accuracies.append(accuracy)
  ```

  accuracies罗列交叉检验的结果

- ```python
  k_to_accuracies[k] = accuracies
  ```

  k_to_accuracies字典k对应精确度列表

## decision boundary

决策边界，我们这么认为在knn样本空间中一定范围内的测试样本应当归属于同一类，分隔这些类型的边界就是决策边界