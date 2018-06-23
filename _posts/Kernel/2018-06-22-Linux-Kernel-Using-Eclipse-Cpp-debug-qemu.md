---
layout: post
title: Eclipse调试Qemu源码
date: 2018-06-22 22:46:30 +08:00
category:
    - Linux内核
keywords: Qemu, Eclipse
tags:
    - Qemu
---

# 下载Eclipse for C/C++工具

首先，下载Eclipse for C/C++，用于调试Qemu源码

下载地址: [eclipse-cpp-photon-RC3-linux-gtk-x86_64.tar.gz](http://ftp.yz.yamagata-u.ac.jp/pub/eclipse//technology/epp/downloads/release/photon/RC3/eclipse-cpp-photon-RC3-linux-gtk-x86_64.tar.gz)

下载下来后，进行解压

> $ tar zxvf eclipse-cpp-photon-RC3-linux-gtk-x86_64.tar.gz

进入eclipse目录下，直接执行eclipse可执行文件即可。

# Eclipse中导入Qemu源码

步骤如下:

**步骤1**:
进入qemu源码所在目录，执行如下配置命令

> $ ./configure \-\-prefix=~/opt/qemu \-\-target-list=\"arm-softmmu i386-softmmu x86_64-softmmu arm-linux-user i386-linux-user x86_64-linux-user\"


**步骤2**: 打开Eclipse后，执行File -> import -> C/C++ -> Existing Code as Makefile Project. 点击Next.

![eclipse-import-qemu-src](/images/Linux_Kernel/eclipse-import-qemu-src.png)

**步骤3**: 选择Qemu源码所在的目录，工具链选择Linux GCC. 如下图

![eclipse-gcc-select](/images/Linux_Kernel/eclipse-gcc-select.png)

点击 **Finish**

**步骤4**: 执行Project -> build project

可能会出现找不到autoreconf工具，那么安装如下package即可。

> $ sudo apt-get install autoconf libtool


**步骤5**: 构建完成后，在Qemu源码目录下的arm-softmmu和x86_64-softmmu下生成对应的qemu可执行文件qemu-system-arm和qemu-system-x86_64

# Eclipse下Qemu执行Linux Kernel

**步骤1**: 执行run -> Run Configurations, 弹出如下对话框， 选择 **C/C++ Application**, 找到qemu-system-arm所在目录，如下图所示

![qemu-system-arm-path-config](/images/Linux_Kernel/qemu-system-arm-path-config.png)

**步骤2**: 选择 **Arguments** 页， **Program arguments** 中填写如下参数，表示qemu-system-arm程序允许后面携带的参数

![qemu-system-arm-run-kernel-argument](/images/Linux_Kernel/qemu-system-arm-run-kernel-argument.png)

**步骤3**: 执行Run -> Run, 则qemu就开始运行Linux系统了

# 参考

- [VNC server running on 127.0.0.1](http://www.crifan.com/qemu/_test/_arm/_vnc/_server/_running/_on/_127/_0/_0/_1/_5900/_no/_other/_output)
