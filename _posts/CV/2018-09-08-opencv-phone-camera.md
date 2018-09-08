---
layout: post
title: Ubuntu下python3+opencv3控制Android手机摄像头
date: 2018-09-08 09:43:10 +08:00
category:
    - 计算机视觉
keywords: opencv
tags:
    - 计算机视觉
---

# 安装ip摄像头App

在进行opencv3控制android手机摄像头之前，首先需要在手机上安装一个App: **[ip摄像头](http://app.mi.com/details?id=com.shenyaocn.android.WebCam)**

然后打开，打开后的界面如下图所示

![ip_camera_open_status.jpeg](/images/cv/ip_camera_open_status.jpeg)

其中的ip是下面程序中所需要的

# 程序控制

以下通过python程序对该ip摄像头进行控制，保存为android_camera.py

```python
import cv2

ESC = 27

if __name__ == '__main__':
    cv2.namedWindow('camera', 1)

    # 打开android摄像头
    video = "http://admin:admin@192.168.1.101:8081/"
    capture = cv2.VideoCapture(video)

    num = 0
    while True:
        succ, img = capture.read()
        cv2.imshow("camera", img)
        key = cv2.waitKey(10)

        # 注：按键是在PC弹出的摄像头显示界面按下，
        if key == ESC:
            print("ESC Key to Quit")
            break
        if key == ord(' '):
            num = num + 1
            #保存图片
            imgFile = "frame_%s.jpg" % num
            cv2.imwrite(imgFile, img)

    capture.release()
    cv2.destroyWindow("camera")
```

# 运行结果

运行以上程序

> python android_camera.py

结果如下

![ip_camera_ctrl_result.jpeg](/images/cv/ip_camera_ctrl_result.jpeg)

# 参考

[opencv3+python 用电脑调用手机的摄像头](https://blog.csdn.net/SHAOYEZUIZUISHAUI/article/details/80959590)
