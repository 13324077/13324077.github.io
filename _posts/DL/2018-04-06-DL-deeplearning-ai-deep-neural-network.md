---
layout: post
title: Deep Learning —— 深度神经网络
date: 2018-04-06 10:02:00 +08:00
category:
    - 深度学习
keywords: DL
tags:
    - 深度学习
---

# 深度神经网络概述

## 什么是深度神经网络

![what-is-deep-nn.png](/images/deep-learning-ai/what-is-deep-nn.png)

图(1) —— 逻辑回归，可以认为是最简单的神经网络

图(2) —— 单隐藏层神经网络，认为是浅层神经网络

图(3) —— 双隐藏层神经网络

图(4) —— 5隐藏层神经网络，可以认为是简单的深度神经网络

那么，在选择深度神经网络结构时，选择多少层的隐藏层是比较合适的，这个隐藏层的个数也是一个可以自由选择的超参数，需要在交叉验证数据集上对不同的隐藏层数进行验证评估，然后得到一个比较合适的层数

## 深度神经网络符号标记

![deep-nueral-network-notation.png](/images/deep-learning-ai/deep-nueral-network-notation.png)

上图是一个4层的深度神经网络结构图，以下使用标记法对该结构进行说明

$$L$$：用来表示神经网络的层数，此处$$L=4$$

$$n^{[l]}$$:表示第$$l$$层的节点数量，例如

- $$n^{[1]} = 5$$表示第一个隐藏层的节点个数为5。

- $$n^{[2]} = 5$$表示第二个隐藏层的节点个数为5。

- $$n^{[3]} = 5$$表示第三个隐藏层的节点个数为3。

- $$n^{[4]} = n^{[L]} = 1$$表示输出层的节点个数为1。

- $$n^{[0]} = n_x = 3$$表示输入层的输入特征值个数为3

$$a^{[l]} = g^{[l]}(z^{[l]})$$表示第$$l$$层的激活函数

$$W^{[l]}$$和$$b^{[l]}$$表示用来计算$$z^{[l]}$$的权值

$$X = a^{[0]}$$: 输入层的输入特征值

$$\hat{y} = a^{[L]}$$: 输出层的预测输出

# 深度神经网络中前向传播

深度神经网络中前向传播的计算过程如下图所示

![deep-nueral-network-notation-forward-propagration.png](/images/deep-learning-ai/deep-nueral-network-notation-forward-propagration.png)

# 深度神经网络矩阵维度确认

对于如下深度神经网络的维度确认规则如下图所示：

![deep-nn-dimension-verification.png](/images/deep-learning-ai/deep-nn-dimension-verification.png)

维度确认的一般规则如下:

$$
\begin{align}
&W^{[l]}:(n^{[l]}, n^{[l-1]}) \\
&b^{[l]}:(n^{[l]}, 1) \\
&dW^{[l]}:(n^{[l]}, n^{[l-1]}) \\
&db^{[l]}:(n^{[l]}, 1) \\
\end{align}
$$

其中 $$n^{[l]}$$表示第$$l$$层的节点个数

# 深度神经网络为什么要深？

首先，深度神经网络在计算什么？，以下以**人脸识别**为例进行介绍

![why-deep-nn-to-deep-face-recognition-example.png](/images/deep-learning-ai/why-deep-nn-to-deep-face-recognition-example.png)

首先进行 **边缘** 检测

然后进行鼻子，眼睛，下巴之类的局部特征检测

最后进行整个脸部的检测

通过这种方式最终识别出人脸

这种深度网络，一开始的几层学习一些低层次的特征，后面的几层就把这些简单的特征结合起来探测更加复杂的特征信息。例如脸部识别，一开始几层学习到的是边缘信息，随着层数的逐渐增多，后面几层就开始探测面部信息了。

# 搭建深度神经网络
