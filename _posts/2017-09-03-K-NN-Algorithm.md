---
layout: post
title: k-NN算法
date: 2017-09-03 15:16:30 +08:00
category: 机器学习
keywords: python, ML, kNN
tags: 机器学习
---

* content
{:toc}

## 图像分类问题

图像分类问题，即对于给定的固定分类集中选择一个类别赋给输入的图像，表示该图像所属的类型。图像分类问题是计算机视觉领域核心问题之一，尽管简单，但是有各种各样的实际应用。

另外，计算机视觉领域的很多其他问题（像对象检测，图像分割等）可以简化为图像分类问题。

### 图像分类例子

对于下图，图像分类器模型对输入的图像，对4个类别{cat, dog, hat, mug}分别计算概率。

对于图像，计算机是使用大的3维数字数组来表示。在该例中，该猫图像是248像素宽，400像素高。有红，绿，蓝三种颜色（简称RGB）表示。因此，图像由248x400x3个数字组成，即总共297600个数字。各个数字是0(黑色)和255(白色）之间的整数。

对于图像分类来说，其任务就是把这百万个数字转换单个的标签（类别），基于本例中的"cat"

![image_classification_example](/images/cs231n/image_classification_example.png)

### 图像分类存在的挑战

对于人类来说，识别一个可视化的图像是非常简单的任务，但是从计算机视觉算法的角度，图像分类问题需要考虑到存在的挑战。如下列出了存在的挑战（不完整），记住图像的原始表示是一个3维的亮度值数组。

**存在的挑战：**

- 视角变化：对于一个对象实例，相对于相机有多个方向

- 尺度变化：可视化类常常在大小方面会有变化

- 变形：许多实体对象并不是刚体，在极端情况下会变形

- 阻挡：实体对象可能会被阻挡，有时只有对象的一小部分可见

- 光照条件：光照效果在像素级别的影响是很大的

- 背景杂斑：实体对象会和环境混合在一起，导致对象很难识别

- 类别之间变化：识别的类别常常相对非常广泛，像椅子，有很多不同的对象类型，各个都有它们自己的外貌。


对于一个好的图像分类模型来说， 以上可变部分的变化并不会导致分类结果的变化，且同时保持类别之间变化的敏感性。

下图是对应的一些可变特性对应的实例

![image_classification_challege_variation](/images/cs231n/image_classification_challege_variation.png)

## 数据驱动方法介绍

以上介绍了图像分类模型存在的挑战，那么我们如何开发一个算法用于把图像分类到不同的类别？ 该算法与对一个数字列表进行排序算法不同。它不是明显地的写一个算法用于识别图像中的猫。因此，我们提供给计算机很多各个类别的例子，然后开发学习算法，并通过查看这些例子学习到各个类别的可视化外貌，该方法称之为**数据驱动方法**，因为该方法依赖与首次累计的带标签图像的训练数据集。

### 图像分类管道(pipeline)

我们已经看到，图像分类任务就是把单个图像的像素数组输入到分类模型中，然后分类模型给该图像分配一个标签（类别）。那么一个完整的图像分类管道如下：

- **输入（input）**：我们的输入是一个N幅图像组成，对于各个图像，我们从k的不同类别中选择一个作为该图像的标签，所以，该输入称之为*训练集*。

- **学习（learning）**: 使用训练集来学习各个别类的样子，我们称该步骤为*训练一个分类器*或*学习一个模型*

- **评估（evaluation）**：最后，我们需要评估训练的分类器的质量，对我们未曾见过的新的图像集预测它们的类别，通过比较这些图像的真实的类别和预测的类别，直观上，我们希望预测的结果与实际的真实答案（"ground truth"）匹配的图像越多，表示分类模型越好。


## 最近邻分类器

作为第一个图像分类方法，我们开发一个称之为最近邻分类器算法（Nearest Neighbor Classifier）,

该算法与卷积神经网络无关，但是实践中很少使用。

但是该分类器方法让我们了解到图像分类问题中基本方法。

### 图像分类数据集

我们的图像分类例子中，使用数据集：**CIFAR-10** 。

[CIFAR-10数据集](http://www.cs.toronto.edu/~kriz/cifar.html)是一个非常流程的玩具级别的图像分类数据集。该数据集由60,000个小的图像（32个像素高，32个像素宽）组成。各个图像的类别是从10个类别（例如，“飞机，汽车，鸟等”）之一。

这些图像分成训练集（50,000个图像）和测试集（10,000个图像）。

下图展示了10个类别中对应的每一类随机选择的10幅图像。

![image_classification_cifar_10_dataset](/images/cs231n/image_classification_cifar_10_dataset.png)

假设，我们给定50,000幅图像CIFAR-10训练集（一共10个类别，每个类别5,000幅图像），然后我们对剩下的10,000幅图像进行分类。

最近邻分类器输入一个测试图像，然后和训练集图像中的每个图像进行比较，预测与训练集中最近的类别。

### 图像比较方法
**那么我们如何比较两幅图像呢？**

一种最简单的方法就是逐个像素的比较，并把所有的差值相加。换句话说，就是给定两幅图像，分别用向量$I_1$和$I_2$表示，那么一种合理的选择就是使用L1距离来进行比较：

$$d_1 (I_1, I_2) = \sum_{p} \left| I^p_1 - I^p_2 \right|$$

以下通过一个简单的实例，可视化该计算过程

![image_classification_visualized_L1_distance](/images/cs231n/image_classification_visualized_L1_distance.png)


### 最近邻算法代码实现

对于分类算法的实现，按如下步骤实现。

**加载CIFAR-10数据到内存中**

数据包含4个数组：训练数据/标签和测试数据/标签。如下代码中，Xtr中保存所有的训练集图像，Ytr表示相应的1维数组保存训练集标签(从0到9,一共10个）。

```python
Xtr, Ytr, Xte, Yte = load_CIFAR10('data/cifar10/') # a magic function we provide
```

同时我们把所有的图像拉伸为1维的向量，如下代码

```python
Xtr_rows = Xtr.reshape(Xtr.shape[0], 32 * 32 * 3) # Xtr_rows becomes 50000 x 3072
Xte_rows = Xte.reshape(Xte.shape[0], 32 * 32 * 3) # Xte_rows becomes 10000 x 3072
```

这样，我们调用训练方法进行训练

```python
nn = NearestNeighbor() # create a Nearest Neighbor classifier class
nn.train(Xtr_rows, Ytr) # train the classifier on the training images and labels
```

利用训练好的分类器对测试集进行预测，并计算预测的正确率

```python
Yte_predict = nn.predict(Xte_rows) # predict labels on the test images
# and now print the classification accuracy, which is the average number
# of examples that are correctly predicted (i.e. label matches)
print 'accuracy: %f' % ( np.mean(Yte_predict == Yte) )
```

注意：作为评估标准，通常使用正确率（accuracy）来度量预测正确的部分。

所有构建的分类器都具有一个通用的API: train(X,y). 该API输入数据和标签，并从其学习。同时有一个predict(X)函数，用于输入新的测试数据，预测它们的类别。如下是一个简单最近邻分类器的实现，使用L1距离。

```python
import numpy as np

class NearestNeighbor(object):
    def __init__(self):
        pass

    def train(self, X, y):
        """ X is N x D where each row is an example. Y is 1-dimension of size N """
        # the nearest neighbor classifier simply remembers all the training data
        self.Xtr = X
        self.ytr = y

    def predict(self, X):
        """ X is N x D where each row is an example we wish to predict label for """
        num_test = X.shape[0]
        # lets make sure that the output type matches the input type
        Ypred = np.zeros(num_test, dtype = self.ytr.dtype)

        # loop over all test rows
        for i in xrange(num_test):
            # find the nearest training image to the i'th test image
            # using the L1 distance (sum of absolute value differences)
            distances = np.sum(np.abs(self.Xtr - X[i,:]), axis = 1)
            min_index = np.argmin(distances) # get the index with smallest distance
            Ypred[i] = self.ytr[min_index] # predict the label of the nearest example

        return Ypred
```

### k-近邻算法

## 验证集
