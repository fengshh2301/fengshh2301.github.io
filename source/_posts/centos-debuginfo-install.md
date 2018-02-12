---
title: centos下gdb调试出现Missing separate debuginfos问题
date: 2018-02-12 16:38:04
tags: [centos,调试]
categories: [贴士]
---

在CentOS7.3下使用gdb进行调试的时候，<!-- more -->

使用bt(breaktrace)命令时，弹出如下信息：

```shell
Missing separate debuginfos, use: debuginfo-install glibc-2.17-196.el7_4.2.x86_64 libgcc-4.8.5-16.el7_4.1.x86_64 libstdc++-4.8.5-16.el7_4.1.x86_64
```

所以直接按照提示执行下列命令：

```shell
sudo debuginfo-install glibc-2.17-196.el7_4.2.x86_64 libgcc-4.8.5-16.el7_4.1.x86_64 libstdc++-4.8.5-16.el7_4.1.x86_64

```

如果不能直接执行，请先执行下面：

```shell
vi /etc/yum.repos.d/CentOS-Debuginfo.repo
# enable=0 -> enable=1
sudo yum install glibc
```

