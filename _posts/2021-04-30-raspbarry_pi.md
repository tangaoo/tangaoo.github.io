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
{% endraw %}