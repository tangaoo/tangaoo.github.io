---
layout: post
title:  "Linux 为何不能硬实时"
categories: Linux
tags:  rt
author: tangoo
mathjax: true
---


* content
{:toc}

something






{% raw %}


* cup 0    12960 ms
* cup 60%  14960 ms
* cup 90%  34960 ms
* cup 90%  10134 ms SCHED_FIFO 99
* chrt --rr 28429 -p 99 
something


{% endraw %}