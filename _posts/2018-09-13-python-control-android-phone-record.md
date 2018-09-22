---
layout: post
title: python通过adb shell控制android手机应用
date: 2018-09-13 09:36:16 +08:00
category:
    - python
keywords:
tags:
    - python
---

# 通过adb shell 操作Android

**步骤1** 首先列出手机上所有应用信息：

> adb shell dumpsys package > ./package.txt

从中找出你要的APP，重点关注 Activity Resolver Table 下面的内容。另外，看你的应用APK包的名字，搜索关键词

**步骤2** 开启和关闭APP

找到要打开的APP后:

强制关闭应用：

> adb shell am force-stop xxxxxx

启动应用：

> adb  shell am start -n xxxxxx/xxxx


以Huawei mate RS手机录音应用为例

1) 打开录音APP

> adb shell am start -n com.android.soundrecorder/.SoundRecorder

2) 开启/关闭录音

> adb shell input tap 540 1900

此命令是模拟手动点击屏幕， (540, 1900)就是点击的屏幕上的位置。

3) 获取手机上的录音文件

> adb pull /storage/sdcard0/Sounds/新录音\ 001.m4a

4) 获取手机指定目录下文件

> adb shell ls /storage/sdcard0/Sounds

5) 删除手机上的文件

> adb shell rm "/storage/sdcard0/Sounds/新录音\ 001.m4a"

# python脚本控制adb shell方法

主要使用 **os.system()** 发送adb shell命令控制android手机

```python
import os

# 启动APP
os.system('adb shell am start -n com.android.soundrecorder/.SoundRecorder')

# 点击(540, 1900)位置，启动录音
os.system('adb shell input tap 540 1900')

time.sleep(1000)

# 再次点击(540, 1900)位置，停止录音
os.system('adb shell input tap 540 1900')

# 获取录音
os.system('adb pull /storage/sdcard0/Sounds/新录音\ 001.m4a')
```
