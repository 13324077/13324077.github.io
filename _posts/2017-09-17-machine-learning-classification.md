---
layout: post
title: 机器学习之分类算法
date: 2017-09-17 16:51:00 +08:00
category: 机器学习
keywords: python, classification
tags: 机器学习
---

* content
{:toc}

最常用的监督式学习任务是：

- 回归(预测数值)

- 分类(预测类别)


例如，预测房价就是回归问题，使用各个算法，如： 线性回归，决策树，和随机森林等

以下对机器学习中分类算法进行学习

# 数据集MNIST介绍

在介绍分类算法之前，先对MNIST数据集进行简单说明，该数据集是分类算法中常用的数据集。各个图像有一个对应的标签表示该图片的数字。该数据集常用来作为学习机器学习算法的入门数据集，因此称之为机器学习中的"Hello World".

因此，当人们提出一种新的分类算法时，都会在MNIST数据集执行看看算法性能。MNIST数据集有70,000幅手写数字的图片集。

Scikit-Learn模块提供了很多的帮助函数来下载常用的数据集，MNIST是其中之一，如下代码用来获取MNIST数据集

```python
from sklearn.datasets import fetch_mldata

mnist = fetch_mldata('MNIST original')
```
![classification-mnist-dataset-download](/images/ml/classification-mnist-dataset-download.png)


通常从scikit-learn上下载的数据集都有一个相似的字典结构，其中

- **DESCR** key: 描述数据集

- **data** key: 包含每个实例的每一行的数组和每个特征的列

- **target** key: 包含标签的数组

下面代码来查看这些数组

```python
X, y = mnist["data"], mnist["target"]
print (X.shape)
print (y.shape)
```

输出结果为：
> (70000, 784)
>
> (70000,)

从输出结果可知：

一共有70,000幅图片，每个图片有784个特征（各个图片28x28=784个像素），每个特征简单表示一个像素的亮度，从0（白色）到255（黑色）。

现在从数据集中取出一个特征向量，转换为28x28像素的矩阵，使用matplotlib的imshow()函数显示，如下

```python
some_digit = X[36000]
some_digit_image = some_digit.reshape(28,28)

plt.imshow(some_digit_image, cmap=matplotlib.cm.binary, interpolation="nearest")
plt.axis("off")
plt.show()
```

运行结果
![mnist_num_5_show](/images/ml/mnist_num_5_show.png)

上图显示的是一个5.

现在把MNIST数据集分为两部分：测试集和训练集

把前60,000个数据作为训练集，后10,000作为测试集，划分代码如下

```python
X_train, X_test, y_train, y_test = X[:60000], X[60000:], y[:60000], y[60000:]
```

然后对训练集进行重新洗牌， 原因：确保交叉验证的结果是相似的，同时由于有些学习算法对训练数据的顺序敏感，重新洗牌确认没有这个问题

代码如下

```python
shuffle_index = np.random.permutation(60000)
X_train, y_train = X_train[shuffle_index], y_train[shuffle_index]
```

这样就获得了训练分类算法所需的训练集和测试集

# 训练一个二元分类器
