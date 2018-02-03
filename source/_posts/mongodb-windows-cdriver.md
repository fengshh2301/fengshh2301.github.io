---
title: mongodb在windows上c接口编译
date: 2018-01-29 10:10:33
tags: [mongodb,windows]
categories: [贴士]
---

mongodb是个好东西，如果要在c代码中使用，需要编译出c接口。<!-- more -->

根据官方文档过了一遍，这里记录。

### 原文：

cd mongo-c-driver-1.8.1\src\libbson
cmake -G "Visual Studio 14 2015 Win64" \
  "-DCMAKE_INSTALL_PREFIX=C:\mongo-c-driver" \
  "-DCMAKE_BUILD_TYPE=Release" # Defaults to debug builds
msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj
msbuild.exe /p:Configuration=Release INSTALL.vcxproj

cd mongo-c-driver-1.8.1
cmake -G "Visual Studio 14 2015 Win64" \
  "-DENABLE_SSL=WINDOWS" \
  "-DENABLE_SASL=SSPI" \
  "-DCMAKE_INSTALL_PREFIX=C:\mongo-c-driver" \
  "-DCMAKE_PREFIX_PATH=C:\mongo-c-driver" \
  "-DCMAKE_BUILD_TYPE=Release" # Defaults to debug builds

msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj
msbuild.exe /p:Configuration=Release INSTALL.vcxproj

### 实际：

cd mongo-c-driver-1.8.1\src\libbson
cmake -G "Visual Studio 14 2015 Win64" -DCMAKE_INSTALL_PREFIX=E:\workspace\mongo-c-driver -DCMAKE_BUILD_TYPE=Release
msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj
msbuild.exe /p:Configuration=Release INSTALL.vcxproj

cd mongo-c-driver-1.8.1
cmake -G "Visual Studio 14 2015 Win64" -DENABLE_SSL=WINDOWS -DENABLE_SASL=SSPI -DCMAKE_INSTALL_PREFIX=E:\workspace\mongo-c-driver -DCMAKE_PREFIX_PATH=E:\workspace\mongo-c-driver -DCMAKE_BUILD_TYPE=Release

msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj
msbuild.exe /p:Configuration=Release INSTALL.vcxproj