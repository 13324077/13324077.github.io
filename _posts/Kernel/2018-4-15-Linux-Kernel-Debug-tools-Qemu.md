---
layout: post
title: Linux内核调试工具Qemu的安装
date: 2018-04-15 21:02:30 +08:00
category:
    - Linux内核
keywords: Qemu, Linux, Kernel
tags: 
    - Qemu
    - Linux
---

# 下载并安装QEMU

## 下载QEMU

下载Qemu-2.11.1版本，下载地址如下：

[qemu-2.11.1](https://download.qemu.org/qemu-2.11.1.tar.bz2)


## 安装QEMU

### 新建安装QEMU文件夹

在/user/local/src/下新建一个QEMU文件夹，用于安装QEMU

> sudo mkdir /usr/local/src/qemu

### 配置QEMU

> ./configure \-\-prefix=/usr/local/src/qemu \-\-target-list="aarch64-softmmu arm-softmmu i386-softmmu x86_64-softmmu arm-linux-user aarch64-linux-user i386-linux-user x86_64-linux-user" \-\-enable-debug

### 编译QEMU

使用如下命令编译

> make -j8

在该过程中出现了如下错误及解决方法：

**错误1**:

(cd /home/fantasy/Debug_Kernel/qemu-1.7.0/pixman; autoreconf -v --install)
/bin/sh: 1: autoreconf: not found
make: *** [/home/fantasy/Debug_Kernel/qemu-1.7.0/pixman/configure] Error 127

解决方法:

sudo apt-get install autoconf


**错误2**:

configure.ac:75: error: possibly undefined macro: AC_PROG_LIBTOOL
      If this token and others are legitimate, please use m4_pattern_allow.
      See the Autoconf documentation.

解决方法:

缺少libtool库
sudo apt-get install libtool


**错误3**:

error: static declaration of ‘memfd_create’ follows non-static declaration

解决方法：

http://debian.2.n7.nabble.com/Bug-891375-qemu-FTBFS-with-glibc-2-27-error-static-declaration-of-memfd-create-follows-non-static-den-td4284669.html

https://git.qemu.org/?p=qemu.git;a=commit;h=75e5b70e6b5dcc4f2219992d7cffa462aa406af0



### 安装QEMU

> make install

# 编译最小文件系统

下载busybox工具包，下载地址如下

[busybox-1.28.2](https://busybox.net/downloads/busybox-1.28.2.tar.bz2)

利用busybox手工编译一个最小文件系统

## 解压busybox

> wxer@wxer:~/dev_ops/Linux_Kernel_DBG$ tar xvf busybox-1.28.2.tar.bz2 


## 编译busybox

> cd busybox-1.28.2/
>
> export ARCH=arm
>
> export CROSS_COMPILE=arm-linux-gnueabi-
>
> make menuconfig

进入memuconfig之后，配置为静态编译，如下图所示

![busybox_menuconfig_build_busybox_as_static_binary.png](/images/Linux_Kernel/busybox_menuconfig_build_busybox_as_static_binary.png)


执行

> make install

进行编译， 完成编译后，在busybox-1.28.2根目录下新增一个**_install**目录，该目录中是编译好的文件系统需要的一些命令集合，如下图所示

![busybox_install_dir_contents.png](/images/Linux_Kernel/busybox_install_dir_contents.png)


## _install中相关操作

进入 **_install**， 先创建etc, dev等目录，命令如下：

> mkdir etc
>
> mkdir dev
>
> mkdir mnt
>
> mkdir -p etc/init.d/

然后在 **_install/etc/init.d/** 目录下新建一个rcS文件，并写入如下内容:

```
mkdir -p /proc
mkdir -p /tmp
mkdir -p /sys
mkdir -p /mnt
/bin/mount -a
mkdir -p /dev/pts
mount -t devpts devpts dev/pts
echo /sbin/mdev > /proc/sys/kernel/hotplug
mdev -s

```

使用chmod命令修改rcS文件权限为可执行权限，

> chmod +x _install/etc/init.d/rcS


在 **_install/etc** 目录下新建一个 **fstab** 文件，并写入如下内容:

```
proc /proc proc defaults 0 0
tmpfs /tmp tmpfs defaults 0 0 
sysfs /sys sysfs defaults 0 0
tmpfs /dev tmpfs defaults 0 0 
debugfs /sys/kernel/debug debugfs defaults 0 0
```

在 **_install/etc** 目录下新建一个 **inittab** 文件，并写入如下内容:

```
::sysinit:/etc/init.d/rcS
::respawn:-/bin/sh
::askfirst:-/bin/sh
::ctrlaltdel:/bin/umount -a -r
```

在 **_install/dev** 目录新建如下设备节点，使用如下命令(**注: 需要root权限**)

> cd _install/dev/
>
> sudo mknod console c 5 1
>
> sudo mknod null c 1 3

最后，把_install目录 复制 到 Linux内核根目录下， 见下文Linux内核编译，其中用到 _install

# Linux内核编译

## 下载

Linux内核版本**4.16.2**，下载地址如下:

[kernel 4.16.2](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.16.2.tar.xz)

## 解压

> xz -d linux-4.16.2.tar.xz
>
> tar -xvf linux-4.16.2.tar

## 配置

使用如下命令进入Linux内核配置界面

> cd linux-4.16.2
>
> export ARCH=arm
>
> export CROSS_COMPILE=arm-linux-gnueabi-
>
> make vexpress_defconfig
>
> make menuconfig

进入配置界面

首先 **配置initramfs**, 在initramfs source file中填入_install, 并把Default kernel command string清空， 如下图

General setup --->

![install_for_initramfs.png](/images/Linux_Kernel/install_for_initramfs.png)

Boot Options --->

![clear_default_kernel_command_string.png](/images/Linux_Kernel/clear_default_kernel_command_string.png)


然后配置memory split为"3G/1G user/kernel/split"， 并打开高端内存，如下图所示

![memory_split.png](/images/Linux_Kernel/memory_split.png)


## 编译

> make bzImage -j8 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

有如下输出信息，表示编译成功

```
  ...
  GEN     .version
  CHK     include/generated/compile.h
  AR      built-in.o
  LD      vmlinux.o
  MODPOST vmlinux.o
  KSYM    .tmp_kallsyms1.o
  KSYM    .tmp_kallsyms2.o
  LD      vmlinux
  SORTEX  vmlinux
  SYSMAP  System.map
  OBJCOPY arch/arm/boot/Image
  Kernel: arch/arm/boot/Image is ready
  LDS     arch/arm/boot/compressed/vmlinux.lds
  AS      arch/arm/boot/compressed/head.o
  GZIP    arch/arm/boot/compressed/piggy_data
  CC      arch/arm/boot/compressed/misc.o
  CC      arch/arm/boot/compressed/decompress.o
  CC      arch/arm/boot/compressed/string.o
  SHIPPED arch/arm/boot/compressed/hyp-stub.S
  SHIPPED arch/arm/boot/compressed/lib1funcs.S
  SHIPPED arch/arm/boot/compressed/ashldi3.S
  SHIPPED arch/arm/boot/compressed/bswapsdi2.S
  AS      arch/arm/boot/compressed/hyp-stub.o
  AS      arch/arm/boot/compressed/lib1funcs.o
  AS      arch/arm/boot/compressed/ashldi3.o
  AS      arch/arm/boot/compressed/bswapsdi2.o
  AS      arch/arm/boot/compressed/piggy.o
  LD      arch/arm/boot/compressed/vmlinux
  OBJCOPY arch/arm/boot/zImage
  Kernel: arch/arm/boot/zImage is ready
```

接着执行如下命令（待确认该命令的用途）

> make dtbs ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

# QEMU中运行Linux内核

执行如下命令:

> qemu-system-arm -M vexpress-a9 -smp 4 -m 1024M -kernel arch/arm/boot/zImage -append "rdinit=/linuxrc console=ttyAMA0 loglevel=8" -dtb arch/arm/boot/dts/vexpress-v2p-ca9.dtb -nographic

执行结果如下

```
audio: Could not init `oss' audio driver
Booting Linux on physical CPU 0x0
Linux version 4.16.2 (wxer@wxer) (gcc version 7.3.0 (Ubuntu/Linaro 7.3.0-16ubuntu2)) #3 SMP Sun Apr 15 22:51:54 CST 2018
CPU: ARMv7 Processor [410fc090] revision 0 (ARMv7), cr=10c5387d
CPU: PIPT / VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
OF: fdt: Machine model: V2P-CA9
Memory policy: Data cache writealloc
On node 0 totalpages: 262144
  Normal zone: 1536 pages used for memmap
  Normal zone: 0 pages reserved
  Normal zone: 196608 pages, LIFO batch:31
  HighMem zone: 65536 pages, LIFO batch:15
random: get_random_bytes called from start_kernel+0xa0/0x3f8 with crng_init=0
percpu: Embedded 16 pages/cpu @(ptrval) s36300 r8192 d21044 u65536
pcpu-alloc: s36300 r8192 d21044 u65536 alloc=16*4096
pcpu-alloc: [0] 0 [0] 1 [0] 2 [0] 3 
Built 1 zonelists, mobility grouping on.  Total pages: 260608
Kernel command line: rdinit=/linuxrc console=ttyAMA0 loglevel=8
log_buf_len individual max cpu contribution: 4096 bytes
log_buf_len total cpu_extra contributions: 12288 bytes
log_buf_len min size: 16384 bytes
log_buf_len: 32768 bytes
early log buf free: 14996(91%)
Dentry cache hash table entries: 131072 (order: 7, 524288 bytes)
Inode-cache hash table entries: 65536 (order: 6, 262144 bytes)
Memory: 1027360K/1048576K available (6144K kernel code, 391K rwdata, 1372K rodata, 3072K init, 182K bss, 21216K reserved, 0K cma-reserved, 262144K highmem)
Virtual kernel memory layout:
    vector  : 0xffff0000 - 0xffff1000   (   4 kB)
    fixmap  : 0xffc00000 - 0xfff00000   (3072 kB)
    vmalloc : 0xf0800000 - 0xff800000   ( 240 MB)
    lowmem  : 0xc0000000 - 0xf0000000   ( 768 MB)
    pkmap   : 0xbfe00000 - 0xc0000000   (   2 MB)
    modules : 0xbf000000 - 0xbfe00000   (  14 MB)
      .text : 0x(ptrval) - 0x(ptrval)   (7136 kB)
      .init : 0x(ptrval) - 0x(ptrval)   (3072 kB)
      .data : 0x(ptrval) - 0x(ptrval)   ( 392 kB)
       .bss : 0x(ptrval) - 0x(ptrval)   ( 183 kB)
SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
Hierarchical RCU implementation.
	RCU event tracing is enabled.
	RCU restricting CPUs from NR_CPUS=8 to nr_cpu_ids=4.
RCU: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=4
NR_IRQS: 16, nr_irqs: 16, preallocated irqs: 16
L2C: platform modifies aux control register: 0x02020000 -> 0x02420000
L2C: DT/platform modifies aux control register: 0x02020000 -> 0x02420000
L2C-310 enabling early BRESP for Cortex-A9
L2C-310 full line of zeros enabled for Cortex-A9
L2C-310 dynamic clock gating disabled, standby mode disabled
L2C-310 cache controller enabled, 8 ways, 128 kB
L2C-310: CACHE_ID 0x410000c8, AUX_CTRL 0x46420001
smp_twd: clock not found -2
sched_clock: 32 bits at 24MHz, resolution 41ns, wraps every 89478484971ns
clocksource: arm,sp804: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 1911260446275 ns
Failed to initialize '/smb@4000000/motherboard/iofpga@7,00000000/timer@12000': -22
Console: colour dummy device 80x30
Calibrating local timer... 100.01MHz.
Calibrating delay loop... 632.01 BogoMIPS (lpj=3160064)
pid_max: default: 32768 minimum: 301
Mount-cache hash table entries: 2048 (order: 1, 8192 bytes)
Mountpoint-cache hash table entries: 2048 (order: 1, 8192 bytes)
CPU: Testing write buffer coherency: ok
CPU0: thread -1, cpu 0, socket 0, mpidr 80000000
Setting up static identity map for 0x60100000 - 0x60100060
Hierarchical SRCU implementation.
smp: Bringing up secondary CPUs ...
CPU1: thread -1, cpu 1, socket 0, mpidr 80000001
CPU2: thread -1, cpu 2, socket 0, mpidr 80000002
CPU3: thread -1, cpu 3, socket 0, mpidr 80000003
smp: Brought up 1 node, 4 CPUs
SMP: Total of 4 processors activated (2519.44 BogoMIPS).
CPU: All CPU(s) started in SVC mode.
devtmpfs: initialized
VFP support v0.3: implementor 41 architecture 3 part 30 variant 9 rev 0
clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
futex hash table entries: 1024 (order: 4, 65536 bytes)
random: fast init done
NET: Registered protocol family 16
DMA: preallocated 256 KiB pool for atomic coherent allocations
cpuidle: using governor ladder
hw-breakpoint: debug architecture 0x4 unsupported.
Serial: AMBA PL011 UART driver
OF: amba_device_add() failed (-19) for /memory-controller@100e0000
OF: amba_device_add() failed (-19) for /memory-controller@100e1000
OF: amba_device_add() failed (-19) for /watchdog@100e5000
irq: type mismatch, failed to map hwirq-75 for interrupt-controller@1e001000!
10009000.uart: ttyAMA0 at MMIO 0x10009000 (irq = 38, base_baud = 0) is a PL011 rev1
console [ttyAMA0] enabled
1000a000.uart: ttyAMA1 at MMIO 0x1000a000 (irq = 39, base_baud = 0) is a PL011 rev1
1000b000.uart: ttyAMA2 at MMIO 0x1000b000 (irq = 40, base_baud = 0) is a PL011 rev1
1000c000.uart: ttyAMA3 at MMIO 0x1000c000 (irq = 41, base_baud = 0) is a PL011 rev1
OF: amba_device_add() failed (-19) for /smb@4000000/motherboard/iofpga@7,00000000/wdt@f000
SCSI subsystem initialized
libata version 3.00 loaded.
usbcore: registered new interface driver usbfs
usbcore: registered new interface driver hub
usbcore: registered new device driver usb
Advanced Linux Sound Architecture Driver Initialized.
clocksource: Switched to clocksource arm,sp804
NET: Registered protocol family 2
tcp_listen_portaddr_hash hash table entries: 512 (order: 0, 6144 bytes)
TCP established hash table entries: 8192 (order: 3, 32768 bytes)
TCP bind hash table entries: 8192 (order: 4, 65536 bytes)
TCP: Hash tables configured (established 8192 bind 8192)
UDP hash table entries: 512 (order: 2, 16384 bytes)
UDP-Lite hash table entries: 512 (order: 2, 16384 bytes)
NET: Registered protocol family 1
RPC: Registered named UNIX socket transport module.
RPC: Registered udp transport module.
RPC: Registered tcp transport module.
RPC: Registered tcp NFSv4.1 backchannel transport module.
hw perfevents: enabled with armv7_cortex_a9 PMU driver, 1 counters available
workingset: timestamp_bits=30 max_order=18 bucket_order=0
squashfs: version 4.0 (2009/01/31) Phillip Lougher
jffs2: version 2.2. (NAND) © 2001-2006 Red Hat, Inc.
9p: Installing v9fs 9p2000 file system support
bounce: pool size: 64 pages
io scheduler noop registered (default)
io scheduler mq-deadline registered
io scheduler kyber registered
clcd-pl11x 10020000.clcd: PL111 designer 41 rev2 at 0x10020000
clcd-pl11x 10020000.clcd: clcd@10020000 hardware, 1024x768@59 display
Console: switching to colour frame buffer device 128x48
clcd-pl11x 1001f000.clcd: PL111 designer 41 rev2 at 0x1001f000
clcd-pl11x 1001f000.clcd: clcd@1f000 hardware, 640x480@59 display
40000000.flash: Found 2 x16 devices at 0x0 in 32-bit bank. Manufacturer ID 0x000000 Chip ID 0x000000
Intel/Sharp Extended Query Table at 0x0031
Using buffer write method
erase region 0: offset=0x0,size=0x40000,blocks=256
40000000.flash: Found 2 x16 devices at 0x0 in 32-bit bank. Manufacturer ID 0x000000 Chip ID 0x000000
Intel/Sharp Extended Query Table at 0x0031
Using buffer write method
erase region 0: offset=0x0,size=0x40000,blocks=256
Concatenating MTD devices:
(0): "40000000.flash"
(1): "40000000.flash"
into device "40000000.flash"
libphy: Fixed MDIO Bus: probed
libphy: smsc911x-mdio: probed
smsc911x 4e000000.ethernet eth0: MAC Address: 52:54:00:12:34:56
isp1760 4f000000.usb: bus width: 32, oc: digital
isp1760 4f000000.usb: NXP ISP1760 USB Host Controller
isp1760 4f000000.usb: new USB bus registered, assigned bus number 1
isp1760 4f000000.usb: Scratch test failed.
isp1760 4f000000.usb: can't setup: -19
isp1760 4f000000.usb: USB bus 1 deregistered
usbcore: registered new interface driver usb-storage
rtc-pl031 10017000.rtc: rtc core: registered pl031 as rtc0
mmci-pl18x 10005000.mmci: Got CD GPIO
mmci-pl18x 10005000.mmci: Got WP GPIO
mmci-pl18x 10005000.mmci: mmc0: PL181 manf 41 rev0 at 0x10005000 irq 34,35 (pio)
ledtrig-cpu: registered to indicate activity on CPUs
input: AT Raw Set 2 keyboard as /devices/platform/smb@4000000/smb@4000000:motherboard/smb@4000000:motherboard:iofpga@7,00000000/10006000.kmi/serio0/input/input0
usbcore: registered new interface driver usbhid
usbhid: USB HID core driver
aaci-pl041 10004000.aaci: ARM AC'97 Interface PL041 rev0 at 0x10004000, irq 33
aaci-pl041 10004000.aaci: FIFO 512 entries
oprofile: using arm/armv7-ca9
NET: Registered protocol family 17
9pnet: Installing 9P2000 support
Registering SWP/SWPB emulation handler
rtc-pl031 10017000.rtc: setting system clock to 2018-04-15 14:56:10 UTC (1523804170)
ALSA device list:
  #0: ARM AC'97 Interface PL041 rev0 at 0x10004000, irq 33
Freeing unused kernel memory: 3072K
input: ImExPS/2 Generic Explorer Mouse as /devices/platform/smb@4000000/smb@4000000:motherboard/smb@4000000:motherboard:iofpga@7,00000000/10007000.kmi/serio1/input/input2


Please press Enter to activate this console. / # 
/ # ls
_install  dev       linuxrc   proc      sys       usr
bin       etc       mnt       sbin      tmp
/ # 

```

# 参考

[用Qemu模拟vexpress-a9 （五） --- u-boot引导kernel，device tree的使用](https://www.cnblogs.com/pengdonglin137/p/5023961.html)
