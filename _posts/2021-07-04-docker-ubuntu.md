---
layout: post
title:  "MAC 上利用 docker 搭建 Ubuntu 编译环境"
categories: docker
tags:  docker
author: tangoo
mathjax: true
---


* content
{:toc}

为了在 MAC 电脑上搭建 Ubuntu 编译环境，用到了 docker 容器。其实虚拟机也可以，但是过于庞大。






{% raw %}

## 1. 安装 docker 

* --cask 代码下载 .dmg 二进制文件安装。

```console
$ brew install --cask docker
```

## 2. 配置 docker

* 参考这个文章，[mac 下使用 Docker 搭建 ubuntu 环境](https://www.smslit.top/2018/12/20/docker_ubuntu_learn/)

## 3. HOST 与 docker 容器 共享文件

```console
docker run -d -p 26122:22 -v /Users/yangmengyun/Desktop/codes:/root/share --name complier ubuntu /usr/sbin/sshd -D
```

## 4. Ubuntu 上安装基本编译环境

* 安装基本编译环境，这一步会把 make gcc 等工具也装上。

```console
apt-get install build-essential
```

* 配置 zsh，参考[文章](https://www.mintimate.cn/2021/02/05/configZsh/)。

{% endraw %}