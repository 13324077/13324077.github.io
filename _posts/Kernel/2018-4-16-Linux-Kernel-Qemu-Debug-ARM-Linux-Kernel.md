---
layout: post
title: QEMU调试ARM Linux内核
date: 2018-04-16 22:54:30 +08:00
category:
    - Linux内核
keywords: Qemu, Linux, Kernel，ARM
tags:
    - Qemu
    - Linux
---

# ARM GDB工具安装

首先，需要安装ARM GDB工具，命令如下

> sudo apt-get install gdb-arm-none-eabi

若以上命令安装不了，按下面的方法安装

- 下载gdb

下载地址:[gdb-8.1](http://ftp.gnu.org/gnu/gdb/gdb-8.1.tar.xz)

- 解压

> xz -d gdb-8.1.tar.xz
>
> tar xvf gdb-8.1.tar

- 配置

> cd gdb-8.1
>
> ./configure \-\-target=arm-linux \-\-program-prefix=arm-linux-gnueabi- \-\-prefix=/usr/local/src/arm-gdb

- 编译

> make -j8
>
> sudo make install

- 添加到PATH路径中

> export PATH=$PATH:/usr/local/src/arm-gdb/bin

- 执行arm-linux-gnueabi-gdb

若出现如下信息，表示安装成功

```
GNU gdb (GDB) 8.1
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=x86_64-pc-linux-gnu --target=arm-linux".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb)

```

# 编译带调试信息内核
然后编译Linux内核，参考[Linux内核调试工具Qemu的安装](https://wxer.github.io/2018/04/15/Linux-Kernel-Debug-tools-Qemu/)中Linux内核编译一节，注意的是需要确保编译的内核中包含调试信息，因此需要添加如下配置

Kernel hacking \-\-\->

Compile-time checks and compiler options  \-\-\->

\[*\]Compile the kernel with debug info    

重新编译内核，编译命令如下：

> make bzImage -j8 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

# 调试过程演示

接着，在一个超级终端中执行如下命令

> qemu-system-arm -nographic -M vexpress-a9 -m 1024M -kernel arch/arm/boot/zImage -append "rdinit=/linuxrc console=ttyAMA0 loglevel=8" -dtb arch/arm/boot/dts/vexpress-v2p-ca9.dtb -S -s

其中

**-S**: 表示QEMU虚拟机会冻结CPU，等到GDB远程输入相应的控制命令

**-s**: 表示在1234端口接受GDB的调试连接

然后，在另一个超级终端中启动ARM GDB，命令如下:

> cd linux-4.16.2           // 进入Linux内核源代码所在目录
>
> arm-linux-gnueabi-gdb \-\-tui vmlinux

如下图所示，GDB接管ARM-Linux内核的运行，并在断点除暂停，此时，可以使用GDB的调试命令来调试内核了。

![arm-gdb-debug-arm-linux.png](/images/Linux_Kernel/arm-gdb-debug-arm-linux.png)

# 参考

- 奔跑吧，Linux内核第6.1.2节
