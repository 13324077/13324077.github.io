---
layout: post
title: ASR —— Kaldi源码下载及安装
date: 2018-03-16 20:23:00 +08:00
category:
    - 语音识别
keywords: ASR
tags:
    - kaldi
---

# 下载kaldi源码

使用如下命令下载

> git clone git@github.com:kaldi-asr/kaldi.git

下载下来的kaldi源码结构如下图所示

![kaldi_src_directory](/images/kaldi/kaldi_src_directory.png)

# 安装kaldi

**进入tools目录**，运行如下命令检查kaldi依赖的软件

> $ extras/check_dependencies.sh

若显示如下内容，则表示kaldi依赖的软件都已安装

```
extras/check_dependencies.sh: all OK.
```

若不是，则需要运行如下命令安装kaldi依赖的软件

> $ make
>
> $ make openblas # 单独安装openblas
>
> $ make openfst # 单独安装openfst
>

可能需要运行如下命令

> $ extras/install_openblas.sh

然后重新检查kaldi依赖的软件是否都已安装

接着返回上一级目录，然后进入 **src目录**， 运行如下命令编译

> $ ./configure \-\-shared \-\-openblas-root=../tools/OpenBLAS/install
>
> $ make clean -j8
>
> $ make depend -j8
>

# kaldi实例

kaldi自带的例子都放在egs目录下

![kaldi_egs_directory](/images/kaldi/kaldi_egs_directory.png)

其中最简单的是yesno这个例子，

进入egs/yesno/s5目录，运行如下命令

> $ sh run.sh

经过一段时间的训练，并测试，运行结果如下

![kaldi_yesno_test.png](/images/kaldi/kaldi_yesno_test.png)

**注:**

```
WER：Word Error Rate的缩写，是单词错误率的意思，用于度量语音识别系统的准确程度。
其计算公式是WER=(I+D+S)/N，其中I代表被插入的单词个数，D代表被删除的单词个数，S代表被替换的单词个数。
也就是说把识别出来的结果中，多认的，少认的，认错的全都加起来，除以总单词数。这个数字当然是越低越好。
```

# 参考

[Kaldi学习手记（一）：Kaldi的编译安装](http://blog.csdn.net/by21010/article/details/49072699)
