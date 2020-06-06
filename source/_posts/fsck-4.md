---
title: Ubuntu引导fsck报错处理
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-19 18:08:27
photos:
tags: [ubuntu,linux]
categories: [运维]
---
Ubuntu 在引导过程中出现如下报错：

```
[ 1.910988] radeon 0000:01:00.0: VCE init error (-110)
fsck from util-linux 2.26.2
/dev/sda1 contains a file system with errors, check forced.
/dev/sda1: Inodes that were part of a corrupted prphan linked list found.

/dev/sda1: UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY.
        (i.e., without -a or -p options)
fsck exited with status code 4
The root filesystem on /dev/sda1 requires a manual fsck
```

#### 出错原因：
分区损坏，磁盘检测不能通过，可能是因为系统突然断电或其它未正常关闭系统导致。

#### 解决方法：
根据上面的错误提示，可以发现是sda1出错，对sda1进行修复：

```
fsck.ext3 -y /dev/sda1
#修复中...
reboot
```
重启后正常引导进入系统。
