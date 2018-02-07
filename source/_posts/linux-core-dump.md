---
title: linux下dump生成core文件
date: 2018-02-07 23:34:52
tags: [linux,dump]
---

Linux下调试C或c++程序时，出现崩溃，<!-- more -->ls下当前目录，有时会发现没有生成core文件。这是因为系统没有打开生成core dump的设置。做个简单记录。

```shell
ulimit -a #可以查看系统core文件的大小限制（第一行），core文件大小设置为0, 即没有打开core dump设置

ulimit -c 0 #不产生core文件

ulimit -c 100 #设置core文件最大为100k

ulimit -c unlimited #不限制core文件大小
```

此后就可以使用：

```shell
gdb ./test core.10000
```

愉快的调试了。。。