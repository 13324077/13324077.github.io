---
layout: post
title: ASR ——  kaldi yesno 孤立词识别脚本分析
date: 2018-06-28 11:06:00 +08:00
category:
    - 语音识别
keywords: ASR
tags:
    - kaldi
---

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
