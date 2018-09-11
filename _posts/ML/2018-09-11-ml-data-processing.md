---
layout: post
title: 机器学习 —— 数据预处理
date: 2018-09-11 22:36:30 +08:00
category:
    - 机器学习
keywords: ML
tags:
    - 机器学习
---

# 数据预处理步骤

对于学习机器学习算法来说，肯定会涉及到数据的处理，因此一开始，对数据的预处理进行学习

对于数据的预处理，大概有如下几步:

**步骤1 —— 导入所需库**

导入处理数据所需要的python库，有如下两个库是非常重要的两个库，每次必导入

- **numpy**

该库包含数学函数功能的库

- **pandas**

该库用于导入和管理数据集


**步骤2 —— 导入数据集**

数据集通常以 **.csv** 格式进行保存，csv文件是以普通文本的形式存储列表数据，文件中每一行是一个数据记录。

对于csv文件，使用pandas模块中的 **read_cvs** 方法进行读取。

**步骤3 —— 处理丢失数据**

由于实际获取到的数据很少是同一类型的，由于各种原因会导致数据丢失，因此需要处理，以便不会降低机器学习模型的性能。

我们可以使用整列数据中的均值或者中值来替换丢失的数据， python中使用sklearn.preprocessing中的imputer类来完成该任务。

**步骤4 —— 编码分类数据**

分类数据通常包括的分类类型是标签值，例如是"Yes"或"No"， 而不是数值，例如0或1。

由于标签值是不能用在机器学习模型的数学等式中的，因此，需要把标签值转换为数值。

python中使用sklearn.preprocessing库中的LabelEncoder类可以完成该任务。

**步骤5 —— 划分数据为训练集和测试集**

机器学习中，需要把数据集划分为两部分，用于训练机器学习模式的称之为 **训练集**, 用于测试训练出来的模型性能的称之为 **测试集**。通常按80/20比例把需数据集划分为训练集和测试集。

python中使用sklearn.crossvalidation库中的train_test_split()方法进行划分。

**步骤6 —— 特征缩放**

大部分的机器学习算法在计算过程中使用两个数据点之间的欧几里德距离。如果数据集中的特征值的变化范围比较大的话， 大的数值比小的数值在计算距离上会导致不同的权重。因此需要进行特征标准化或Z-score正规化。

python中可以使用sklearn.preprocessing库中的StandardScalar


# 代码实现

```python
# -*- coding: utf-8 -*-
"""
Author: wxer
"""
# step 1 - import the libraries
import numpy as np
import pandas as pd
from sklearn.preprocessing import Imputer
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
from sklearn.cross_validation import train_test_split
from sklearn.preprocessing import StandardScaler


# step 2 - import dataset
dataset = pd.read_csv('Data.csv')
X = dataset.iloc[:, :-1].values
Y = dataset.iloc[:, 3].values


# step 3 - handing the missing data
imputer = Imputer(missing_values='NaN', strategy='mean', axis=0)
imputer = imputer.fit(X[:, 1: 3])
X[:, 1: 3] = imputer.transform(X[:, 1: 3])


# step 4 - encoding  categorical data
labelencoder_X = LabelEncoder()
X[:, 0] = labelencoder_X.fit_transform(X[:, 0])

onehotencoder = OneHotEncoder(categorical_features=[0])
X = onehotencoder.fit_transform(X).toarray()
labelencoder_Y = LabelEncoder()
Y = labelencoder_Y.fit_transform(Y)


# step 5 - splitting the datasets into training sets and test sets
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=0)

# step 6 - feature scaling

sc_X = StandardScaler()
X_train = sc_X.fit_transform(X_train)
X_test = sc_X.fit_transform(X_test)

```



# 参考

1. [Data PreProcessing](https://github.com/Avik-Jain/100-Days-Of-ML-Code/blob/master/Code/Day%201_Data%20PreProcessing.md)

2. [三种常用数据标准化方法](https://blog.csdn.net/bbbeoy/article/details/70185798)
