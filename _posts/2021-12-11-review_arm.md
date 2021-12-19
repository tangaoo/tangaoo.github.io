---
layout: post
title:  "Review ARM"
categories: ARM
tags:  ARM
author: tangoo
mathjax: true
---


* content
{:toc}

Review ARM architecture 笔记。





{% raw %}

## 1. 历史分类

* MPU， 8086时代叫 MPU。
* CPU，功能单一，必须外接 RAM、Storage。
* MCU，除运算功能外，还集成了片上 RAM、Storage等外设，这样使得电路设计简单，但 MCU 一般针对专有市场。
* APU，除集成了片上 RAM、Storage等外设，还能外扩大容量 RAM、Storage。能上大型系统。

## 1. 编译
### 1.1 下载源码

```console
$ git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```

### 1.2 配置编译环境

```console
$ bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```


{% endraw %}
