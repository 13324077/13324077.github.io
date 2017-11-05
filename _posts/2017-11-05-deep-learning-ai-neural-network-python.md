---
layout: post
title: 神经网络编程之Python
date: 2017-11-05 08:53:30 +08:00
category: 机器学习
keywords: python, ML
tags: python
---

* content
{:toc}

# 向量化(Vectorization)

向量化可以使计算的速度更快。

那么什么是向量化呢？

例如对于$$z= w^T x +b$$, 其中$$w\in \Re^{n_x}$$, $$x\in \Re^{n_x}$$, 即都是$$n_x$$维的列向量

那么，**非向量化** 的实现如下：

```
z = 0
for i in range(n_x)
    z += w[i] * x[i]
z +=b
```

**向量化实现**：
```
z = np.dot(w,x) + b
```

实际用python实现对比发现，向量化实现的运行时间比非向量化快很多。 以下以实际python实现进行对比。

```python
import time
import numpy as np

a = np.random.rand(1000000)
b = np.random.rand(1000000)

# 向量化实现
tic = time.time()
c = np.dot(a, b)
toc = time.time()

print(c)
print("Vectorized version:" + str(1000*(toc-tic)) + "ms")

# 非向量化实现
c = 0
tic = time.time()
for i in range(1000000):
    c += a[i] * b[i]
toc = time.time()

print(c)
print("For loop: " + str(1000*(toc-tic)) + "ms")
```

运行结果：

```
249860.941919
Vectorized version:1.9125938415527344ms
249860.941919
For loop: 359.036922454834ms
```

从结果可以看出，向量化实现运行时间快将近300倍。因此在后续的机器学习算法实现中，尽量使用向量化方式实现。

不管是GPU还是CPU，，都有SIMD(Single Instruction Multiple Data)指令, 当使用np的内置函数像np
.dot()，都是针对运行进行了并行化计算的处理，因此运行更快。

因此，在任何时候，都要避免使用for运行进行计算。

# 向量化例子

尽量使用np中内置的函数，例如，对于向量
$$
\begin{gather}
v =
{
  \begin{bmatrix}
  v_1\\
  v_2\\
  \vdots\\
  v_n
  \end{bmatrix}
}
\end{gather}
$$

对向量中每个数进行指数运算，则调用np.exp(v)

log运算，调用np.log(v)

绝对值运算，调用abs(v)

获取最大值运算，调用np.maximum(v, 0)

平方运算，调用v**2

倒数运算，调用1/v

如下是一个逻辑回归中非向量化实现转换为向量化实现的例子

![logistic-regression-derivatives-from-unvectorization-to-vectorization](/images/deep-learning-ai/logistic-regression-derivatives-from-unvectorization-to-vectorization.png)

# 逻辑回归向量化实现

## 前向传播向量化

对于训练集中有m个训练样本时，对于每一个样本都要进行如下计算

第一个样本：

$$z^{(1)} = w^T x^{(1)} +b\\
a^{(1)} = \sigma(z^{(1)})$$

第二个样本：

$$
z^{(2)} = w^T x^{(2)} +b\\
a^{(2)} = \sigma(z^{(2)})
$$

...

第m个样本：

$$
z^{(m)} = w^T x^{(m)} +b\\
a^{(m)} = \sigma(z^{(m)})
$$

...

若用for循环实现，那么要循环m次。

以下介绍如何使用向量化实现

前面介绍过使用 **X** 来表示m个训练样本

$$
\begin{gather}
X =
\left.
\underbrace
{
  \begin{bmatrix}
  |&& |&& \cdots&& |\\
  x^{(1)}&& x^{(2)}&& \cdots&& x^{(m)}\\
  |&& |&& \cdots &&|
  \end{bmatrix}
}_{m}
  \right \}~{n_x}
\end{gather}
$$

该矩阵是一个$$n_x \times m$$大小的矩阵

那么可以通过如下一步完成计算
$$
Z = [z^{(1)}, z^{(2)}, ..., z^{(m)}] = w^T X + [b, b,...,b]
=[w^T x^{(1)} + b, w^T x^{(2)} + b,..., w^T x^{(m)} + b]
$$

以上计算，在Python中通过一句代码即可实现，如下：

```python
z = np.dot(w.T, X) + b
```

然后使用$$A = [a^{(1)},a^{(2)},...,a^{(m)}]=\sigma(z)$$计算得到A的值。


## 梯度下降向量化

通过前面学习我们知道

$$
dz^{(1)} = a^{(1)} - y^{(1)}\\
dz^{(2)} = a^{(2)} - y^{(2)}\\
...
$$

向量化后为：

$$
dz = [dz^{(1)},dz^{(2)},..., dz^{(m)}]
$$

同时由前面的反向传播求导可知

$$A = [a^{(1)}, a^{(2)},..., a^{(m)}]$$, $$Y = [y^{(1)}, y^{(2)}, ..., y^{(m)}]$$

$$dz = A - Y = [a^{(1)} -y^{(1)},a^{(2)} -y^{(2)},...,a^{(m)} -y^{(m)}]$$,

=>

从而dw的计算如下：

$$
dw = 0\\
dw += x^{(1)}dz^{(1)}\\
dw += x^{(2)}dz^{(2)}\\
...\\
dw += x^{(m)}dz^{(m)}\\
dw/=m
$$

db的计算如下：

$$
db = 0\\
db += x^{(1)}dz^{(1)}\\
db += x^{(2)}dz^{(2)}\\
...\\
db += x^{(m)}dz^{(m)}\\
db/=m
$$

因此，向量化表示如下

$$
db = \frac{1}{m} \sum^{m}_{i=1}dz^{(i)} = \frac{1}{m} np.sum(dz)
$$

$$
dw = \frac{1}{m}X dz^T

= \frac{1}{m}\begin{bmatrix}
|&& |&& \cdots&& |\\
x^{(1)}&& x^{(2)}&& \cdots&& x^{(m)}\\
|&& |&& \cdots &&|
\end{bmatrix}
\begin{bmatrix}
dz^{(1)}\\
\vdots\\
dz^{(m)}\\
\end{bmatrix}\\

= \frac{1}{m}[x^{(1)}dz^{(1)} + x^{(2)}dz^{(2)} + ... + x^{(m)}dz^{(m)}]
$$

从而得到$$n \times 1$$的dw向量

完整的梯度下降迭代计算向量化如下图所示

![gradient-descent-vectorization](/images/deep-learning-ai/gradient-descent-vectorization.png)


# python广播机制

先通过一个例子解释下广播机制

对如下矩阵

$$
\begin{gather}
X =
  \begin{bmatrix}
  56.0 && 0.0&& 4.4&& 68.0\\
  1.2 && 104.0&& 52.0&& 8.0\\
  1.8 && 135.0&& 99.0&& 0.9\\
  \end{bmatrix}
\end{gather}
$$

进行如下计算，分别对矩阵的四列求和，并将矩阵的每个元素都除以对应列的和，

最简单的方法是通过for循环进行计算。但是在python中通过进行矩阵操作和广播机制即可实现，python代码如下

![python-broadcast-example](/images/deep-learning-ai/python-broadcast-example.png)

代码解析：

axis = 0表示对列进行求和，axis = 1表示对行进行求和

其中
```python
percentage = 100 * A / cal.reshape(1, 4)
```
使用到了python的广播机制， 用一个3x4的矩阵A除以一个1x4的矩阵， 实际上3x4矩阵的每一行都会对应的处理1x4的矩阵，这就是广播机制

其他的广播机制的机制

**例子1** - 对于如下

$$
\begin{gather}
v =
{
  \begin{bmatrix}
  1\\
  2\\
  3\\
  4
  \end{bmatrix}
} + 100
\end{gather}
$$

在python中使用的广播机制，实际上对应的计算为：

$$
\begin{gather}
v =
{
  \begin{bmatrix}
  1\\
  2\\
  3\\
  4
  \end{bmatrix}
} +
{
  \begin{bmatrix}
  100\\
  100\\
  100\\
  100
  \end{bmatrix}
}
=
{
  \begin{bmatrix}
  101\\
  102\\
  103\\
  104
  \end{bmatrix}
}
\end{gather}
$$

**例子2** - 对于如下矩阵

$$
{
  \begin{bmatrix}
  1 && 2 && 3\\
  4 && 5 && 6
  \end{bmatrix}
}
+
{
  \begin{bmatrix}
  100 && 200 && 300
  \end{bmatrix}
}
$$

利用python的广播机制，实际对应的计算为：

$$
{
  \begin{bmatrix}
  1 && 2 && 3\\
  4 && 5 && 6
  \end{bmatrix}
}
+
{
  \begin{bmatrix}
  100 && 200 && 300\\
  100 && 200 && 300
  \end{bmatrix}
}=
{
  \begin{bmatrix}
  101 && 202 && 303\\
  104 && 205 && 306
  \end{bmatrix}
}
$$

**例子3** —— 对于如下计算

$$
{
  \begin{bmatrix}
  1 && 2 && 3\\
  4 && 5 && 6
  \end{bmatrix}
}
+
{
  \begin{bmatrix}
  100\\
  200
  \end{bmatrix}
}
$$

利用python的广播机制，实际对应的计算为

$$
{
  \begin{bmatrix}
  1 && 2 && 3\\
  4 && 5 && 6
  \end{bmatrix}
}
+
{
  \begin{bmatrix}
  100 && 100 && 100\\
  200 && 200 && 200
  \end{bmatrix}
}=
{
  \begin{bmatrix}
  101 && 102 && 103\\
  204 && 205 && 206
  \end{bmatrix}
}
$$

**关于Python中广播机制的一般规则**:

1. $$m \times n$$矩阵 与 $$1\times n$$或$$m \times 1$$矩阵进行加减乘除运算时，$$1\times n$$或$$m \times 1$$矩阵都转换为$$m \times n$$矩阵进行运算。

2. 对于$$m \times 1$$矩阵与实数进行加减乘除时，都转换为$$m \times 1$$维矩阵进行运算。
