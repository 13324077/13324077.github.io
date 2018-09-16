---
layout: post
title: 机器学习 —— libsvm库的使用
date: 2018-09-16 19:12:30 +08:00
category:
    - 机器学习
keywords: ML
tags:
    - 机器学习
    - SVM
---

# libsvm库简介

libsvm是一个实现支持向量机(C-SVC, nu-SVC), 支持向量回归(epsilon-SVR, nu-SVR)和分布估计(one-class SVM)的集成软件， 且支持多类别分类。
由国立台湾大学林智仁(Chih-Jen Lin)开发。

[libsvm网站](https://www.csie.ntu.edu.tw/~cjlin/libsvm/)

libsvm软件下载地址:[https://www.csie.ntu.edu.tw/~cjlin/libsvm/](https://www.csie.ntu.edu.tw/~cjlin/libsvm/)

下载的libsvm压缩包文件夹包括如下文件

![libsvm_software_folder.png](/images/ml/libsvm_software_folder.png)

libsvm的文件组织结构说明如下:

- **主文件路径**：包含了核心的C/C++程序和例子数据。其中svm.cpp是svm的核心程序，它实现了svm的训练和测试算法。

- **tool子文件路径**：包含了一些检验数据格式以及选择svm参数的tool。

- **其他子文件路径**：主要包含pre-built 二值文件和相关语言的接口。

# windows下libsvm用法

**步骤1 下载数据库**

下载地址:

[svmguide1](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary/svmguide1)

[svmguide1.t (testing)](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary/svmguide1.t)

**步骤2 使用缺省参数基于原始数据集进行训练和预测**

**训练**

> E:\libsvm\libsvm-3.23>.\windows\svm-train.exe svmguide1.txt

输出

```
...*..*
optimization finished, #iter = 5371
nu = 0.606150
obj = -1061.528918, rho = -0.495266
nSV = 3053, nBSV = 722
Total nSV = 3053
```

**预测**

> E:\libsvm\libsvm-3.23>.\windows\svm-predict.exe svmguide1.t svmguide1.txt.model
svmguide1.t.predict

输出

```
Accuracy = 66.925% (2677/4000) (classification)
```

**步骤3 先对数据进行缩放，再用缺省参数进行训练和测试**

**数据缩放**

训练数据缩放

> E:\libsvm\libsvm-3.23>.\windows\svm-scale.exe -l -1 -u 1 -s range1 svmguide1.txt
 > svmguide1.scale

测试数据缩放

> E:\libsvm\libsvm-3.23>.\windows\svm-scale.exe -r range1 svmguide1.t > svmguide1.
t.scale

**训练**

> E:\libsvm\libsvm-3.23>.\windows\svm-train.exe svmguide1.scale

输出

```
*
optimization finished, #iter = 496
nu = 0.202599
obj = -507.307046, rho = 2.627039
nSV = 630, nBSV = 621
Total nSV = 630
```

**预测**

> E:\libsvm\libsvm-3.23>.\windows\svm-predict.exe svmguide1.t.scale svmguide1.scal
e.model svmguide1.t.predict

输出

```
Accuracy = 96.15% (3846/4000) (classification)
```

由以上可见，通过对数据进行缩放操作后， 分类的正确率有了很大的提升（从66.925%提升到96.15%）



# 参考

1. [A Practical Guide to Support Vector Classification](https://www.csie.ntu.edu.tw/~cjlin/papers/guide/guide.pdf)

2. [libsvm代码阅读：基础准备与svm.h头文件](https://blog.csdn.net/linj_m/article/details/19498747)
