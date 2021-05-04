---
layout: post
title:  "Raspbarry Pi 开箱指南"
categories: Raspbarry
tags:  Raspbarry
author: tangoo
mathjax: true
---


* content
{:toc}

Raspbarry Pi 开箱指南记录。






{% raw %}

## 1. 下载系统镜像

* 下载地址 https://shumeipai.nxez.com/download#os

## 2. 制作 SD卡

* 下载制作 SD卡工具Win32DiskImager，地址 https://sourceforge.net/projects/win32diskimager/files/Archive/

## 3. 配置 SSH连接（无屏幕）
### 3.1 有网线直连(静态连接)

* 找到 SD卡 boot目录下面cmdline.txt。添加“ip=xxxx.xxxx.xxxx.xxxx”

### 3.2 无线连接（有路由器）

* 在 boot目录下新建 wpa_supplicant.conf文件，文件里填如下内容。
  ```
    country=CN
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    
    network={
    ssid="WiFi-A"
    psk="12345678"
    key_mgmt=WPA-PSK
    priority=1
    }
  ```

### 3.3 开启 ssh

* 在 boot目录下，新建一个文件，命名为 ssh 即可。

## 4. 基本配置（命令行）

```
  sudo raspi-config
```

## 5. 查看 raspbarry pi 引脚

```
  pinout
```

## 6. 编译替换内核

* [官方内核编译参考](https://www.raspberrypi.org/documentation/linux/kernel/building.md)
* [参考链接](https://www.raspberrypi.org/documentation/linux/kernel/building.md)

## 7. 打实时补丁

* [参考链接](https://lemariva.com/blog/2019/09/raspberry-pi-4b-preempt-rt-kernel-419y-performance-test)
* [已编译完成二进制包](https://github.com/lemariva/RT-Tools-RPi/tree/master/preempt-rt/kernel_4_19_59-rt23-v7l%2B)
* 跑 `sudo cyclictest --mlockall --smp --priority=80 --interval=200 --distance=0`
  * 未打实时补丁，max 达到 400us
  * 打实时补丁，max 达到 93us
  * 打了实时补丁后，明显感觉到相同任务情况下，CPU 占有率变高了不少。


{% endraw %}