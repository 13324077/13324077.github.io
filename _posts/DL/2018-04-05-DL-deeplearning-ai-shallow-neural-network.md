---
layout: post
title: Deep Learning —— 浅层神经网络
date: 2018-04-05 17:30:00 +08:00
category:
    - 深度学习
keywords: DL
tags:
    - 深度学习
---

以下介绍如何实现一个神经网络，


# 神经网络表示

浅层神经网络，即只有一个隐藏层的神经网络结构， 如下图

![shallow-nn.png](/images/deep-learning-ai/shallow-nn.png)

在一个神经网络中，当使用监督学习训练它时，训练集包含了输入$$x$$和目标输出$$y$$

上图中

$$a^{[0]} = X$$: 输入层

$$a^{[1]}$$: 隐藏层，

$$a^{[2]}$$: 输出层

$$W^{[1]}$$的维度4x3， $$b^{[1]}$$的维度4x1

$$W^{[2]}$$的维度1x4, $$b^{[2]}$$的维度1x1

# 神经网络的输出

对于之前学习的逻辑回归，如下图

![logistic-regression-vs-shallow-nn.png](/images/deep-learning-ai/logistic-regression-vs-shallow-nn.png)

左边的圆圈表示逻辑回归计算的两个步骤：

- 首先计算出$$z = w^T x + b$$

- 然后计算激活函数$$a = \sigma{(z)}$$

从右边的图可知，神经网络只是重复逻辑回归的计算多次。

如下介绍如何进行多次的逻辑回归的计算得到最后的神经网络

先介绍下神经网络中的每个节点的计算都是一个逻辑回归的计算步骤，如下图

![shallow-nn-explaination-by-logistic-regression.png](/images/deep-learning-ai/shallow-nn-explaination-by-logistic-regression.png)

其中$$a_{i}^{[l]}$$的$$[l]$$表示神经网络的第$$l$$层，$$i$$表示第$$l$$层的第$$i$$个节点。

这样，整个第$$l$$层的每个节点的如下图所示

![shallow-nn-calc-by-logistic-regression-method.png](/images/deep-learning-ai/shallow-nn-calc-by-logistic-regression-method.png)

上面的计算方式，通过代码实现时，需要通过for循环计算每个节点的值，当节点增多时，计算效率非常低，因此使用向量化的方式进行处理，如下图所示

![shallow-nn-calc-by-vectorization.png](/images/deep-learning-ai/shallow-nn-calc-by-vectorization.png)

总结一下，对于下面左图的神经网络，只需要下面右图中的四个公式，即可计算最终的神经网络输出

![shallow-nn-output-calc.png](/images/deep-learning-ai/shallow-nn-output-calc.png)

以上是针对单个训练样本计算神经网络的输出，那么对于多个训练样本呢，以下以$$m$$个训练样本为例，介绍计算方法，如下图所示

![multiple-example-nn-output-calc.png](/images/deep-learning-ai/multiple-example-nn-output-calc.png)

其中$$a^{[l][(i)]}$$中$$[l]$$表示神经网络的第$$l$$层，$$i$$表示第l层的第$$i$$个样本，

上图中，对于$$m$$个样本，通过for循环来计算每一个样本对应的值。

但是，由于for循环的计算效率低，因此需要转换为向量化的方式进行计算

转换方式如下

![multiple-example-nn-output-calc-for-to-vectorization.png](/images/deep-learning-ai/multiple-example-nn-output-calc-for-to-vectorization.png)

其中

$$X$$: $$n_x$$x$$m$$矩阵

$$W^{[1]}$$: 4x$$n_x$$矩阵

$$b^{[1]}$$: 4x1矩阵

$$a^{[1]}$$: 4x$$m$$矩阵, 4表示隐藏层节点个数

$$W^{[2]}$$: 1x4矩阵

$$b^{[2]}$$: 1x1矩阵

$$a^{[2]}$$: 1x$$m$$矩阵

以下对这种向量化的方式进行直观的解释，如下图所示

![shallow-nn-multiple-example-vectorization-explaination.png](/images/deep-learning-ai/shallow-nn-multiple-example-vectorization-explaination.png)

向量化最终的结果如下图

![multiple-example-nn-output-vectorization-recap.png](/images/deep-learning-ai/multiple-example-nn-output-vectorization-recap.png)

从上面可以看出，神经网络中每一层的计算过程都是一样的，如上图中

第一层的计算过程如下

$$
Z^{[1]} = W^{[1]}X +b^{[1]}
$$

$$
A^{[1]} = \sigma(Z^{[1]})
$$

由于$$X$$也用$$A^{[0]}$$表示，所以

$$
Z^{[1]} = W^{[1]}A^{[0]} +b^{[1]}
$$

第二层的计算过程如下：

$$
Z^{[2]} = W^{[2]}A^{[1]}+b^{[2]}
$$

$$
A^{[2]} = \sigma(Z^{[2]})
$$

由上可以，除了输入特征值和层编号改变外，计算过程都是一样的。

后续的深度网络，无法也是重复这两步的计算，只是网络的层数更加的多而已。

# 激活函数

搭建神经网络时，我们可以选择隐藏层使用哪一种激活函数，以及输出层使用哪一种激活函数。

前面介绍的神经网络使用的激活函数都是sigmoid函数，然后这并不是最优的选择，以下介绍各种不同的激活函数。

## sigmoid函数

该函数公式如下:

$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

形状如下

![sigmoid-activate-function.png](/images/deep-learning-ai/sigmoid-activate-function.png)

在神经网络的正向传播计算中，有如下两步

$$z^{[1]} = W^{[1]}x + b^{[1]}$$

$$a^{[1]} = \sigma(z^{[1]})$$

其中$$sigma(z^{[1]})$$这一步就是使用的sigmoid激活函数，

>注：
>
> 对于sigmoid激活函数，除非用在二元分类的输出层，不然绝对不要用

以下介绍其他激活函数

## tanh函数

该激活函数为:

$$
tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}
$$

形状如下：

![tanh-activate-function.png](/images/deep-learning-ai/tanh-activate-function.png)

tanh激活函数在隐藏层更优越于sigmoid函数。

但是在输出层例外，因为对于分类问题，一般$$y\in{0,1}$$， 所以希望$$0 \leq \hat{y} \leq 1$$

然后sigmoid函数和tanh函数都有一个 **缺点**: 当z非常大或者非常小的时候，导数的梯度或者说是斜率可能非常小，斜率几乎接近与0, 这样会导致梯度下降算法的收敛速度变得很慢， 以下介绍另一种激活函数解决该问题 —— **ReLU** 函数

## ReLU函数

ReLU —— Rectified Linear Unit的缩写

该函数的公式如下

$$
ReLU(z) = a = max(0, z)
$$

形状如下

![ReLu-activate-function.png](/images/deep-learning-ai/ReLu-activate-function.png)

> 在选择激活函数是有一些经验法则：
>
> - 如果输出值为0或1, 例如二元分类问题， 那么sigmoid函数很适合作为输出层的激活函数
>
> - 其他所有单元都使用ReLU函数
>
> - 在训练神经网络时，当隐藏层不知道使用哪个激活函数时，可以选择使用ReLU激活函数

ReLU激活函数有个缺点，当z为负值时，导数等于0，然后在实践中这并没有什么问题。

但ReLU还有另一个变种，叫Leaky ReLU，

## Leaky ReLU

Leaky ReLU与ReLU的区别是当z为负数时，导致不再为0, 它有一个较平缓的协议，因此称之为Leaky ReLU。

通常Leaky ReLU比ReLU激活函数好，然后实际中使用的并不多。

该函数的公式如下

$$
Leaky ReLU(z) = a = max(0.01z, z)
$$

形状如下

![Leaky-ReLU-activate-function.png](/images/deep-learning-ai/Leaky-ReLU-activate-function.png)

深度学习网络有一个特点，就是在建立神经网络是经常会有很多不同的选择，像

- 隐藏层单元数

- 激活函数

- 初始化权重

- ...

然后，很难定一个准则来确定什么参数最适合某个问题， 那么建议时：

当不知道哪一种激活函数最有效时，可以先试试，在交叉验证数据集上对比验证下，看看哪个效果更好，就用哪个。

# 为什么需要非线性激活函数

事实证明，要让神经网络能够计算出有趣的函数，必须使用非线性激活函数，

如下图所示

![why-non-linear-activate-function.png](/images/deep-learning-ai/why-non-linear-activate-function.png)

当激活函数使用线性激活函数替代时，例如替换为$$g(z) = z$$ —— 线性激活函数（恒等激活函数），通过右边的推导可知，这样的神经网络的输出就是把输入进行线性组合，这样就和没有任何隐藏层的标准逻辑回归是一样的了。

对于这种对于$$y$$的输出是连续的实数而非{0,1}可能是有用的。

# 激活函数导数
