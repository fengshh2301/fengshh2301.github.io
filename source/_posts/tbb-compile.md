---
title: tbb编译
date: 2018-01-30 00:25:41
tags: [tbb,编译]
categories: [贴士]
---

程序中需要使用无锁队列，故引用tbb库，做下记录以防遗忘。<!-- more -->

```shell
#去到tbb库根目录
cd tbb
make
#或者windows系统下
msbuild /P:Configuration=Debug /P:Outdir="../../lib/Debug" tbb.vcxproj
msbuild /P:Configuration=Release /P:Outdir="../../lib/Release" tbb.vcxproj
```

