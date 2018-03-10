---
title: linux查看内存
date: 2018-03-11 00:48:20
tags: [centos]
categories: [贴士]
---

linux系统中经常查看内存占用情况<!-- more -->，有下面几种方式：

```shell
top
free -m
ps -e -o 'pid,comm,args,pcpu,rsz,vsz,stime,user,uid' | grep oracle | sort -nrk6
```