---
layout:     post
title:      "周志遠教授作業系統 第2講"
subtitle:   "Operating System Concept"
date:       2025-01-26
author:     "PCLiu"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - OS
---

# 周志遠教授作業系統 第2A講 Ch0:Historical Prospective

## Multi-programming system (最初的系統)

- CPU 跟 I/O 可以重疊，讓CPU使用率提高
- Spooling (Simultaneous Peripheral operation On-Line)
- 多個Job同時Load到memory裡面，讓CPU去多工執行 (multiplexing)

![scheduling](/img/in-post/2025-01-26-course-operating-system-02/scheduling.png)

**Time-sharing System (新版的系統)**

- Interactive between user and system
    - CPU Frequently switches between I/O (keyboard/screen) and jobs immediately
- Multiple user can share a single computer
- Switch job between
    - A short period of time
- 把CPU切成很多個Time Slot，讓User感覺Jobs是同時在執行
- OS 需要做的事情
    - Virtual memory (disk 的空間): 讓memory塞越多的程式越好，提升CPU使用率
    - File system & Disk management
    - Process synchronization & Deadlock

**Summary of Multi-programming System**

![summary](/img/in-post/2025-01-26-course-operating-system-02/summary.png)

# 周志遠教授作業系統 第2B講 Ch0:Historical Prospective

## Computer System Architecture

**Desktop Systems: Personal Computer** 
- Single user
- Single CPU
- Convenient & Responsiveness GUI
- I/O devices
- OS: Windows, Mac OS, Linux

**Parallel Systems** 
- Multiple CPUs or tighly coupled system
- Communicate via shared memory
- Symmetric Multiprocessing (SMP)
  - 每個CPU core都是對等的
  - 最常見的多核心系統
- Asymmetric Multiprocessing
  - 每一個CPU core有不同的task
  - 一個CPU core是Master，其他的是Slave
  - Large systems
- Many-Core Processor
  - GPU: Single instruction, multiple data (SIMD)

## References

[1] [11001周志遠教授作業系統｜第2A講 Syllabus&Ch0: Historical Prospective](https://www.youtube.com/watch?v=ko-AxqDZ1Mw)

[2] [11001周志遠教授作業系統｜第2B講 Ch0: Historical Prospective](https://www.youtube.com/watch?v=gJ-xUHwWEeo)


如果您喜歡本篇文章，請繼續關注我的Blog :)