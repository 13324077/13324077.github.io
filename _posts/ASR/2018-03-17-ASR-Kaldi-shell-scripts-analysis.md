---
layout: post
title: ASR —— kaldi脚本分析
date: 2018-03-17 10:50:00 +08:00
category:
    - 语音识别
keywords: ASR
tags:
    - kaldi
---

对于kaldi语音识别系统，都是通过shell脚本以及perl脚本完成

- 语料库的下载，
- 数据的准备，
- 词典准备，
- 语言模型准备
- 特征提取
- 模型训练
- 解码

以下分别进行介绍

以下以 **egs/yesno/s5** 中的脚本为例，对整个语音识别系统的过程进行说明

# path.sh脚本分析

```shell
export KALDI_ROOT=`pwd`/../../..
[ -f $KALDI_ROOT/tools/env.sh ] && . $KALDI_ROOT/tools/env.sh
export PATH=$PWD/utils/:$KALDI_ROOT/tools/openfst/bin:$PWD:$PATH
[ ! -f $KALDI_ROOT/tools/config/common_path.sh ] && echo >&2 "The standard file $KALDI_ROOT/tools/config/common_path.sh is not present -> Exit!" && exit 1
. $KALDI_ROOT/tools/config/common_path.sh
export LC_ALL=C
```

- 添加kaldi主目录路径

- 如果存在env.sh文件，则执行该脚本

- 添加openfst执行文件等目录路径

- 如果不存在common_path.sh文件，则打印报错，退出执行

- 若存在，则执行该脚本文件

- 按照C排序法则

# 语料库下载

下载语料库的脚本如下

```shell
if [ ! -d waves_yesno ]; then
  wget http://www.openslr.org/resources/1/waves_yesno.tar.gz || exit 1;
  # was:
  # wget http://sourceforge.net/projects/kaldi/files/waves_yesno.tar.gz || exit 1;
  tar -xvzf waves_yesno.tar.gz || exit 1;
fi
```

- 首先判断 **waves_yesno** 目录存不存在，若存在就不用重新下载

- 然后使用wget下载waves_yesno.tar.gz压缩文件

- 最后使用tar进行解压，得到waves_yesno

执行完后，在当前目录下新增 **waves_yesno** 目录

![01-download-wave-yesno-data](/images/kaldi/01-download-wave-yesno-data.png)

**waves_yesno** 目录中是 **.wav** 语音文件

![kaldi_yesno_wav_files](/images/kaldi/kaldi_yesno_wav_files.png)

总共60个wav文件，采样率都是8k，

wav文件里每一个单词要么”ken”要么”lo”(“yes”和”no”)的发音，所以每个文件有8个发音，文件命名中的1代表yes发音，0代表no的发音。

# 准备数据

有如下数据需要准备，

- 语料库

- 词典

- 语言模型


## 语料库准备

```shell
local/prepare_data.sh waves_yesno
```

其中 **prepare_data.sh** 的内容如下

```shell
#!/bin/bash

mkdir -p data/local
local=`pwd`/local
scripts=`pwd`/scripts

export PATH=$PATH:`pwd`/../../../tools/irstlm/bin

echo "Preparing train and test data"

train_base_name=train_yesno
test_base_name=test_yesno
waves_dir=$1  # 传入的文件目录赋值给waves_dir变量

ls -1 $waves_dir > data/local/waves_all.list  # 将waves_dir目录所有文件名写到waves_all.list文件中

cd data/local

# 将waves_all.list中的60个wav文件名,分成两拨，各30个，分别记录在waves.test和waves.train文件中
../../local/create_yesno_waves_test_train.pl waves_all.list waves.test waves.train

../../local/create_yesno_wav_scp.pl ${waves_dir} waves.test > ${test_base_name}_wav.scp

../../local/create_yesno_wav_scp.pl ${waves_dir} waves.train > ${train_base_name}_wav.scp

# 生成train_yesno.txt和test_yesno.txt
# 这两个文件存放的是发音id和对应的文本．
../../local/create_yesno_txt.pl waves.test > ${test_base_name}.txt

../../local/create_yesno_txt.pl waves.train > ${train_base_name}.txt

cp ../../input/task.arpabo lm_tg.arpa

cd ../..

# This stage was copied from WSJ example
for x in train_yesno test_yesno; do
  mkdir -p data/$x
  cp data/local/${x}_wav.scp data/$x/wav.scp
  cp data/local/$x.txt data/$x/text
  cat data/$x/text | awk '{printf("%s global\n", $1);}' > data/$x/utt2spk
  utils/utt2spk_to_spk2utt.pl <data/$x/utt2spk >data/$x/spk2utt
done
```

执行完以上shell脚本后，在当前目录下新增如下内容

![02-prepare-yesno-data](/images/kaldi/02-prepare-yesno-data.png)

## 词典准备

```shell
local/prepare_dict.sh
```

执行完该脚本后，当前目录下新增如下内容

![03-prepare-yesno-dict](/images/kaldi/03-prepare-yesno-dict.png)

## 语言模型准备

```shell
utils/prepare_lang.sh --position-dependent-phones false data/local/dict "<SIL>" data/local/lang data/lang
```

![04-prepare-yesno-lang-1](/images/kaldi/04-prepare-yesno-lang-1.png)

![04-prepare-yesno-lang-2](/images/kaldi/04-prepare-yesno-lang-2.png)

```shell
local/prepare_lm.sh
```

![05-prepare-yesno-lm](/images/kaldi/05-prepare-yesno-lm.png)

# 特征提取

```shell
for x in train_yesno test_yesno; do
 steps/make_mfcc.sh --nj 1 data/$x exp/make_mfcc/$x mfcc
 steps/compute_cmvn_stats.sh data/$x exp/make_mfcc/$x mfcc
 utils/fix_data_dir.sh data/$x
done
```

# 模型训练

```shell
steps/train_mono.sh --nj 1 --cmd "$train_cmd" \
  --totgauss 400 \
  data/train_yesno data/lang exp/mono0a
```

# 解码

```shell
# Graph compilation  
utils/mkgraph.sh data/lang_test_tg exp/mono0a exp/mono0a/graph_tgpr

# Decoding
steps/decode.sh --nj 1 --cmd "$decode_cmd" \
    exp/mono0a/graph_tgpr data/test_yesno exp/mono0a/decode_test_yesno

for x in exp/*/decode*; do [ -d $x ] && grep WER $x/wer_* | utils/best_wer.sh; done
```


# 参考

- [kaldi-yesno-tutorial](https://github.com/keighrim/kaldi-yesno-tutorial)

- [OpenFst Library](http://www.openfst.org/twiki/bin/view/FST/WebHome)

- [Corpus Phonetics Tutorial - Kaldi](https://www.eleanorchodroff.com/tutorial/kaldi/kaldi-prereq.html)

- [Some Kaldi Notes](http://jrmeyer.github.io/asr/2016/02/01/Kaldi-notes.html)

- [kaldi中FST的可视化-以yesno为例](http://blog.csdn.net/u013677156/article/details/77893661)

- [走进语音识别中的WFST（一）](http://blog.csdn.net/l_b_yuan/article/details/50876340)
