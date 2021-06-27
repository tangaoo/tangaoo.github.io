---
layout: post
title:  "Git Submodule 子模块用法"
categories: git
tags:  git
author: tangoo
mathjax: true
---


* content
{:toc}

记录 git submodule 在实际项目中的应用，以 [uSPF](https://github.com/tangaoo/uSPF) 项目举例，uSPF 项目中包含了 [ttlib_micro](https://github.com/tangaoo/ttlib_micro) 项目。这么做的好处显而易见，保持了两个项目的独立，uSPF 项目又能很方便的拿到 ttlib_micro 项目的最新版本，同时也不影响 ttlib_mirco 在其他项目中的应用。






{% raw %}

## 1. 添加远程子模块

将 ttlib_mircro 远程项目克隆到本地 uSPF 项目中。
```console
$ cd uSPF
$ git submodule add https://github.com/tangaoo/ttlib_micro.git ttlib_micro
```
添加完子模块后，会多出来一个 .gitmodules 文件, 这是用来保存子模块的信息。

## 2. 查看子模块

```console
$ git submodule
```

## 3. 更新子模块

* 更新下项目内子模块到最新版本。
```console
$ git submodule update
```

* 更新下项目内子模块在远程版本中的最新版本。
```console
$ git submodule update --remote
```

## 4. 克隆包含子模块的完整项目

* 递归克隆父项目以及子项目。
```console
$ git clone https://github.com/maonx/vimwiki-assets.git assets --recursive 
```

## 5. 修改子模块

* 修改子模块后，直接提交到远程分支。
```console
$ git add .
$ git ci -m "commit"
$ git push origin HEAD:master
```

{% endraw %}