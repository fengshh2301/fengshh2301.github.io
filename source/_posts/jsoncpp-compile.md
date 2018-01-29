---
title: jsoncpp编译
date: 2018-01-30 00:24:56
tags: [jsoncpp,编译]
categories: [贴士]
---

由于要在cxx中解析json，所以需要用到jsoncpp库，在此做个简单记录。<!-- more -->

```shell
#去到jsoncpp库根目录
cd jsoncpp
mkdir build
cd build
cmake ..
make
#或者windows系统下
msbuild /P:Configuration=Debug ALL_BUILD.vcxproj /P:Outdir="../../../lib/Debug"
msbuild /P:Configuration=Release ALL_BUILD.vcxproj /P:Outdir="../../../lib/Release"
```

