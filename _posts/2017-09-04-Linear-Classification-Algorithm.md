---
layout: post
title: 线性分类算法
date: 2017-09-04 12:27:30 +08:00
category: 
    - 机器学习
keywords: python, opencv
tags: 
    - 机器学习
---

* content
{:toc}


在k-NN算法中介绍了图像分类的问题，即在固定的类别集合中选择一个类别赋给一幅图像。另外，我们通过使用k-近邻分类算法（kNN）比较测试数据（标注的）图像和训练集以预测测试图像的类别。

但是，kNN有很多缺点：

- 分类器必须记住所有的训练数据，并存储用于与测试数据进行比较。该算法空间利用率低，因此训练数据集可能具有Gigabyte的大小空间。

- 分类一个测试图像的开销比较大，需要和所有的训练图像进行比较。


接下来，我们将开发一个更加强大的方法用于图像分类，然后逐步延伸到整个神经网络和卷积神经网络。

该方法有两个主要的组建：

- score function： 映射原始数据到类别分数

- loss function：对预测的分数和实际的标签之间的量化协议


# 图像到标签分数的参数化映射

该方法的第一个组件是定义一个score function，用于映射一幅图像的像素值为各个类别的置信度分数。

以下以具体的例子开发分类方法。

假设图像训练数据集 $$x_i \in R^D$$, 各个图像对应的标签为$$y_i$$，其中$$i=1...N$$和$$y_i \in 1...K$$，即我们有$$N$$个例子（各个例子的维度为$$D$$)，K表示类别的数目。

例如，对于CIFAR-10数据集中，有$$N=50,000$$幅图像的训练集。各个图像具有$$D=32x32x3=3072$$个像素，以及$$K=10$$，由于有10个不同的类别（狗，猫，汽车等），我们现在定义一个score function $$f: R^D\mapsto R^K$$

## 线性分类器

从最简单的可论证的函数，线性映射开始：

$$f(x_i, W, b) = Wx_{i} + b$$

在上面的等式中，我们假设图像$$x_i$$是把所有的像素了拉伸为一个单列的$$[D\times 1]$$形状的向量。矩阵**$$W$$**(大小为$$[K\times D]$$),和向量**b**(大小为$$[K\times 1]$$)为该函数的参数。

对应到CIFAR-10图像集中，$$x_i$$包含第$$i$$幅图像中所有的像素拉伸为单个$$[3072\times 1]$$的列，矩阵**$$W$$**(大小为$$[10\times 3072]$$),和向量**b**(大小为$$[10\times 1]$$).所以，3072个数值（原始像素值）输入到该函数中，10个类别（类别分数）输出。参数**$$W$$**通常称为**权值**，$$b$$称为**偏置向量**，**$$b$$**会影响输出的分数，但是没有和实际数据$$x_i$$有交互。然而，人们常常会交替的使用术语**权值(weight)**和**参数(parameter)**。

对于线性分类器，有如下一些主要注意：

- 首先，单个的矩阵乘法$$Wx_i$$并行（各个表示一个类别）的有效的评估10个单个的分类器，其中每个分类器就是$$W$$的每一行。

- Notice also that we think of the input data $$(x_i, y_i)$$ as given and fixe, but we have control over the setting of the parameters **$$W,b$$**，Our goal will be to set these in such way that the computed scores match the ground truth labels across the whole training set. We will go into much more details about how this is done, but intuitively we wish that the correct class has a score that is higher than the scores of incorrect classes.

- An advantage of this approach is that the training data is used to learn the parameters **$$W, b$$**, but once the learning is complete we can discard the entire training set and only keep the learned parameters. That is because a new test image can be simply forwarded through the function and classified based on the computed scores.

- Lastly, Note that to classifying the test image involves a single matrix multiplication and addition, which is significantly faster that comparing a test image to all training images.


> 铺垫：卷积神经网络也是按上面所示的方式映射图像像素到类别分数，但是映射函数$$f$$将更加复杂同时包含更多的参数。


# 线性分类器解释

Notice that a linear classifier computer the score of a class as a weighted sum of all of its pixel values across all 3 of its color channels. Depending on precisely what values we set for these weights, the function has the capacity to like or dislike(depending on the sign of each weight) certain colors at certain positions in the image. For instance, you can imagine that the **"ship"** class might be more likely if there is a lot of blue on the sides of an image(which could likely correspond to water). You might expect that the **"ship"** classifier would then have a lot of positive weights across its blue channel weights(presence of blue increases score of ship), and negative weights in the red/green channel(presence of red/green decreases the score of ship).

![linear_classifier_examples](/images/cs231n/linear_classifier_examples.png)

上图中，作为映射一幅图像到类别分数的例子，为了可视化，假设图像仅仅有4个像素（4个单色像素，为了简化，不考虑色彩通道），同时有3个类别（红色（猫），绿色（狗），蓝色（船））。（说明：特别的，此处的颜色简单的指示3个类别，与RGB色彩通道没有关系）， 拉伸图像像素为一个单列并执行矩阵乘法以得到各个类别的分数。注意此处的权值$$W$$并不好，该权值把猫的图像赋给了很低的类别分数，该组权值把该猫图分类为狗。

**Analogy of Image as High-Dimensional Points**.

Since the images are stretched into high-dimensional column vectors, we can interpret each image as a single point in this space (e.g. each image in CIFAR-10 is a point in 3072-dimensional space of 32x32x3 pixels). Analogously, the entire dataset is a (labeled) set of points.

Since we defined the score of each class as a weighted sum of all image pixels, each class score is a linear function over this space. We cannot visualize 3072-dimensional spaces, but if we imagine squashing all those dimensions into only two dimensions, then we can try to visualize what the classifier might be doing:

![linear_classifier_squashing_to_point_for_visualizing](/images/cs231n/linear_classifier_squashing_to_point_for_visualizing.png)

....


# 损失函数(Loss Function)

## Multiclass Support Vector Machine Loss


# 实践考虑

## Delta设置


# Softmax分类器

事实证明，SVM是两种常见的分类器之一，另一个受欢迎的分类器是**Softmax分类器**， 它有一个不同的损失函数，
