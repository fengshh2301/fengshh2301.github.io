---
title: centos��gdb���Ա�: no symbol table
date: 2018-02-12 16:48:38
tags: [centos,����]
categories: [��ʿ]
---

centos����Ҫ���ض��ļ����öϵ�<!-- more -->������gdb��`break filename:linenum`ʱ������

```shell
No source file
#����
No symbol table is loaded.  Use the "file" command.
```

����������ã�

1.makefile�����ã�

```shell
CFLAGS = -g #c����
CPPFLAGS = -g #cpp����
```

cmake���ã�

```shell
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -Wall -g -ggdb")
```

2.����ʱcmakeʹ��debugģʽ������

```shell
cmake -DCMAKE_BUILD_TYPE=Debug .
```

