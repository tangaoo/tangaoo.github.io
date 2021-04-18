---
layout: post
title:  "Linux 进程调度"
categories: Linux 
tags:  schedule
author: tangoo
mathjax: true
---

* content
{:toc}

Linux 的调度策略主要包括 SCHED_OTHER、SCHED_RR、SCHED_FIFO。





{% raw %}

## 1. SCHED_OTHER
### 1.1 机制

* SCHED_OTHER 是 Linux 默认的调度方式，即每个进程轮流使用一段时间 CPU（时间片）。
* 进程无法直接控制自己何时使用 CPU，使用多长时间的 CPU。这样满足了交互式多任务系统的两个重要需求。
  * 公平性，每个进程都有机会得到 CPU。
  * 响应性，一个进程在得到 CPU前无需等待太长时间。

### 1.2 优先级

* nice 值，越高则代表对其他进程越“友好”，即自己的优先级低。
* nice 值是一个权重因素，它导致内核调度器更加偏向调度优先级更高的进程。例如给一个进程赋最低优先级（高 nice值）并不会导致它完全无法得到 CPU，但是导致它得到 CPU的次数会变少。

## 2. SCHED_RR（实时调度）
### 2.1 机制

* 在 SCHED_RR（循环）中，高优先级进程直接抢占低优先级进程，但相同优先级进程之间是以时间片轮转方式调度。 
* 进程发生切换的条件。
  * 到达时间片的终点。（拥有相同优先级的多进程才会有该情况发生）
  * 自愿放弃 CPU，可能是 IO阻塞，或者调用 sched_yield()系统调用。
  * 终止了。
  * 被一个进入了就绪态的高优先进程抢占了。
    * 之前被阻塞的高优先级，解除了阻塞。（如 IO操作完成了）
    * 另一个进程的优先级被提高了。
    * 本优先级被降低了。

### 2.2 优先级

* Linux 提供99个优先级，1最低，99最高。

## 3. SCHED_FIFO（实时调度）
* SCHED_FIFO 策略与 SCHED_RR 一样，唯一的区别就是 SCHED_FIFO没有时间片轮转方式，也就是说在相同优先级多进程切换时，必须等待上一个经常主动退出。

## 4. 实时调度（SCHED_RR、SCHED_FIFO）与 SCHED_OTHER区别
* 实时调度策略存在严格的优先级级别，高优先级的进程总是优先于优先级较低的进程。
* 实时调度策略可以让用户精确地控制多进程之间地调用顺序。

## 5. sched_yield() 
### 5.1 机制

* 如果存在与调用进程的优先级相同的其他排队的可运行进 程，那么调用进程会被放在队列的队尾，队列中队头的进程将会被调度使用 CPU。
* 如果在该优先级队列中不存在可运行的进程，那么 sched_yield()不会做任何事情，调用进程会继续使用 CPU。

## 6. 对于线程

* 线程也具有上述特性，因为线程是调度单位，也可以继承进程特性。

## 7. 代码实践
* 运行时，要给 sudo权限。
* 需要手动把这两个线程绑定到同一个核上面。（多核环境的话）
* 主线程打印任然有效地原因时，非实时线程任然有5%的 CPU使用权，原因如下：
  * `/proc/sys/kernel/sched_rt_period_us` 默认值为 1000000，代表 CPU运行时间宽度。
  * `/proc/sys/kernel/sched_rt_runtime_us` 默认值为 950000，代表实时线程运行时间宽度。也就是非实时线程还有 5%的 CPU。如果把该值设置 -1，则实时线程会占满。 

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#define __USE_GNU
#include <pthread.h>
#include <sched.h>

long long a = 0;
long long b = 0;

int attach_cpu(int cpu_index)
{
    int cpu_num = sysconf(_SC_NPROCESSORS_CONF);
    if (cpu_index < 0 || cpu_index >= cpu_num)
    {
        printf("cpu index ERROR!\n");
        return -1;
    }

    cpu_set_t mask;
    CPU_ZERO(&mask);
    CPU_SET(cpu_index, &mask);

    if (pthread_setaffinity_np(pthread_self(), sizeof(mask), &mask) < 0)
    {
        printf("set affinity np ERROR!\n");
        return -1;
    }

    return 0;
}

void *thread1(void *param)
{
    attach_cpu(0);

    long long i;
    for (i = 0; i < 10000000000; i++)
    {
        a++;
    }
}

void *thread2(void *param)
{
    attach_cpu(0);

    long long i;
    for (i = 0; i < 10000000000; i++)
    {
        b++;
    }
}

int main()
{
    pthread_t t1;
    pthread_t t2;

    int policy;
    struct sched_param param;

    if (pthread_create(&t1, NULL, thread1, NULL) < 0)
    {
        printf("create t1 failed!\n");
        return -1;
    }

    param.sched_priority = 10;
    policy = SCHED_FIFO;
    pthread_setschedparam(t1, policy, &param);

    if (pthread_create(&t2, NULL, thread2, NULL) < 0)
    {
        printf("create t2 failed!\n");
        return -1;
    }

    param.sched_priority = 11;
    policy = SCHED_FIFO;
    pthread_setschedparam(t2, policy, &param);

    while (1)
    {
        printf("a=%lld, b=%lld\n", a, b);
        sleep(1);
    }

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```

{% endraw %}