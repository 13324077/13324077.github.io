---
layout: post
title: ASR —— 语音识别概述(1)
date: 2018-03-04 22:16:30 +08:00
category:
    - 语音识别
keywords: ASR
tags:
    - 语音识别
---

语音识别技术，目前有两大类:

- 传统语音识别技术，

- 现代语音识别技术


# 传统语音识别技术

主要是针对MFCC + HMM + GMM算法组合进行语音识别的技术，传统的语音识别架构如下


# 现代语音识别技术

主要是针对DNN + CTC或DNN + HMM算法组合进行语音识别的技术，这种语音识别技术的架构如下

# 语音识别学习

不管是GMM + HMM 还是DNN + HMM或者是DNN + CTC需要按如下流程进行学习

1. 语音信号处理，主要是特征提取，像MFCC，LPC等

2. 声学模型，像GMM

3. 序列模型，像HMM，RNN，LTSM等

4. 语言模型，像N-Gram

5. 相关的机器学习算法，像决策树，GMM，DNN等

6. 相关算法，—— Vertibe算法，EM算法等

7. 代码学习 —— Kaldi
