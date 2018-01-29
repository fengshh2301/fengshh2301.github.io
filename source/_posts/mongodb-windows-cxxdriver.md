---
title: mongodb在windows上c++接口编译
date: 2018-01-29 10:10:38
tags: [mongodb,windows,cxxdriver]
categories: [贴士]
---

mongodb是个好东西，如果要在c++代码中使用，需要编译出c++接口。<!-- more -->

根据官方文档过了一遍，这里记录。

### 原文：

cmake -DCMAKE_BUILD_TYPE=Release
​    -DCMAKE_INSTALL_PREFIX=/your/cxxdriver/prefix
​    -DLIBMONGOC_DIR=/your/cdriver/prefix
​    -DLIBBSON_DIR=/your/cdriver/prefix ..

msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj

msbuild.exe /p:Configuration=Release INSTALL.vcxproj
​	
​	

### 实际：

3.1以下
cmake  -G "Visual Studio 14 2015 Win64" -DCMAKE_INSTALL_PREFIX=E:\workspace\mongo-cxx-driver -DLIBMONGOC_DIR=E:\workspace\mongo-c-driver -DLIBBSON_DIR=E:\workspace\mongo-c-driver -DBOOST_ROOT=E:\workspace\ftrade\external\boost_1_60_0 -DCMAKE_BUILD_TYPE=Release ..
3.2以上
cmake  -G "Visual Studio 14 2015 Win64" -DCMAKE_INSTALL_PREFIX=E:\workspace\mongo-cxx-driver  -DCMAKE_PREFIX_PATH=E:\workspace\mongo-c-driver -DBOOST_ROOT=E:\workspace\ftrade\external\boost_1_60_0 -DCMAKE_BUILD_TYPE=Release ..

msbuild.exe /p:Configuration=Release ALL_BUILD.vcxproj
msbuild.exe /p:Configuration=Release INSTALL.vcxproj