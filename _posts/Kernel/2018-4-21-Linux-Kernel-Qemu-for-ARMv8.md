---
layout: post
title: QEMU运行ARMv8开发平台
date: 2018-04-20 22:12:30 +08:00
category:
    - Linux内核
keywords: Qemu, Linux, Kernel，ARM
tags:
    - Qemu
    - Linux
---

# 安装ARMv8编译器

Ubuntu系统下运行如下命令安装

> sudo apt-get install gcc-aarch64-linux-gnu

运行 **aarch64-linux-gnu-gcc --version**, 若有如下信息，则表示安装成功

```
wxer@wxer:~$ aarch64-linux-gnu-gcc --version
aarch64-linux-gnu-gcc (Ubuntu/Linaro 7.3.0-16ubuntu2) 7.3.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

# 编译和制作基于aarch64架构最小文件系统

参考[Linux内核调试工具Qemu的安装](https://wxer.github.io/2018/04/15/Linux-Kernel-Debug-tools-Qemu/)编译最小系统方法， 只是使用的编译环境变量不同而已，编译命令如下

> cd busybox-1.28.2/
>
> make menuconfig ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-

修改make install的目的路径名为 **\_install\_arm64**，如下图

![busybox_intall_destination_path_modify_content.png](/images/Linux_Kernel/busybox_intall_destination_path_modify_content.png)

![busybox_intall_destination_path_modify.png](/images/Linux_Kernel/busybox_intall_destination_path_modify.png)

同时配置为构建为静态二进制文件，如下图

![busybox_menuconfig_build_busybox_as_static_binary.png](/images/Linux_Kernel/busybox_menuconfig_build_busybox_as_static_binary.png)

然后执行如下命令

> make install

生成\_install\_arm64文件, 拷贝到linux-4.16.2目录下

然后进入Linux内核源码目录下，编译linux内核，

> cd linux-4.16.2/
>
> make menuconfig ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-

General setup \-\-\->

配置修改为:

![kernel_initramfs_source_file_install_arm64.png](/images/Linux_Kernel/kernel_initramfs_source_file_install_arm64.png)

Kernel Features \-\-\->

Page Size(4KB) \-\-\->

配置修改为:

![kernel-feature-page-size-virtual-address-space-size-48bits.png](/images/Linux_Kernel/kernel-feature-page-size-virtual-address-space-size-48bits.png)

然后执行如下命令编译内核

> make -j8 ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-

# QEMU运行ARMv8

执行如下命令

> qemu-system-aarch64 -machine virt -cpu cortex-a57 -machine type=virt -nographic -m 2048 -smp 2 -kernel arch/arm64/boot/Image -append "rdinit=/linuxrc console=ttyAMA0 loglevel=8"

若没有错误，则运行结果如下:

```

```
