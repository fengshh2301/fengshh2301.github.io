---
title: centos下gdb调试报: no symbol table
date: 2018-02-12 16:48:38
tags: [centos,调试]
categories: [贴士]
---

centos下想要给特定文件设置断点<!-- more -->，但是gdb中`break filename:linenum`时，报错：

```shell
No source file
#或者
No symbol table is loaded.  Use the "file" command.
```

检查两个设置：

1.makefile中设置：

```shell
CFLAGS = -g #c代码
CPPFLAGS = -g #cpp代码
```

cmake设置：

```shell
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -Wall -g -ggdb")
```

2.启动时cmake使用debug模式构建：

```shell
cmake -DCMAKE_BUILD_TYPE=Debug .
```

