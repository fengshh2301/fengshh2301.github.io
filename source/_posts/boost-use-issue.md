---
title: boost使用问题
date: 2018-04-15 17:14:42
tags: [boost]
categories: [贴士]
---

在引用boost库的过程中<!-- more -->，会出现下面这个警告：

> Please define _WIN32_WINNT or _WIN32_WINDOWS appropriately. For example:
>
> - add -D_WIN32_WINNT=0x0501 to the compiler command line; or
> - add _WIN32_WINNT=0x0501 to your project's Preprocessor Definitions.
>   Assuming _WIN32_WINNT=0x0501 (i.e. Windows XP target).

怎么解决呢？

原因是在引用boost头文件之前，没有先引用windows的系统版本头文件SDKDDKVer.h，在这里只要保证引用boost之前，先引用该文件就可以得到正确的windows版本，如下：

```c++
#ifdef _WIN32
#include <SDKDDKVer.h>
#else
#endif
```

