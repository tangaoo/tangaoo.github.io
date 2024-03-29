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

* 网线连接路由器即可，会自动分配ip。

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

### 3.3 获取 raspberrypi 的ip
* 登陆路由器后台，即可获取到接入设备的ip。
* 或者在PC机上执行 `arp -a` 扫描网段上所有设备，此时把 raspberrypi 开关机一次即可判断出其ip。

### 3.4 开启 ssh

* 在 boot 目录下，新建一个文件，命名为 ssh 即可，默认用户名 pi，密码 raspberry。
```shell
ssh pi@192.168.3.200
```

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
* 在树莓派 boot 盘的 config.txt 文件中可以配置选择内核。目前用的是 4.19.71-rt24-v7l+ 版本。

## 7. 打实时补丁

* [参考链接](https://lemariva.com/blog/2019/09/raspberry-pi-4b-preempt-rt-kernel-419y-performance-test)
* [已编译完成二进制包](https://github.com/lemariva/RT-Tools-RPi/tree/master/preempt-rt/kernel_4_19_59-rt23-v7l%2B)
* 跑 `sudo cyclictest --mlockall --smp --priority=80 --interval=200 --distance=0`
  * 未打实时补丁，max 达到 400us
  * 打实时补丁，max 达到 93us
  * 打了实时补丁后，明显感觉到相同任务情况下，CPU 占有率变高了不少，也就是整个吞吐量下降了。
* 打了实时补丁后，查看 menuconfig，会发现有 `Fully Preemptile Kernel` 选项。


{% endraw %}