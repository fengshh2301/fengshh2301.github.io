---
title: tbb编译
date: 2018-01-30 00:25:41
tags: [tbb]
categories: [贴士]
---

程序中需要使用无锁队列，故引用tbb库，做下记录以防遗忘。<!-- more -->

进入tbb库根目录。

#### linux系统

```shell
cd tbb
make
```

#### windows系统

```shell
cd tbb
make
#32位
msbuild /P:Configuration=Debug /P:Outdir="../../lib/Debug" tbb.vcxproj
msbuild /P:Configuration=Release /P:Outdir="../../lib/Release" tbb.vcxproj
#64位
msbuild /P:Configuration=Debug /P:Outdir="../../lib/Debug" /P:Platform=x64 tbb.vcxproj
msbuild /P:Configuration=Release /P:Outdir="../../lib/Release" /P:Platform=x64 tbb.vcxproj
```

