---
title: windows下判断32位和64位程序的编译
date: 2018-02-02 20:51:42
tags: [windows, 编译]
categories: [贴士]
---

在vs编程中，常常涉及到32位和64位程序的编译<!-- more -->，怎么判断当前编译是32位编译还是64位编译？如何判断
是debug下编译还是release下编译？

1.判断debug,release编译

如果_DEBUG定义了表示是debug编译，否则是release编译(也可以使用NDEBUG判断)。

2.判断32位,64位编译

在 Win32 配置下，_WIN32 有定义，_WIN64 没有定义。在 x64 配置下，两者都有定义。即在 VC 下，_WIN32 一定有定义。

因此，WIN32/_WIN32 可以用来判断是否 Windows 系统（对于跨平台程序），而 _WIN64 用来判断编译环境是 x86 还是 x64。附一个表：

| 常量\定义  | 预定义选项 | Windows.h      | VC编译器 |
| ------ | ----- | -------------- | ----- |
| WIN32  | Win32 | √(minwindef.h) | ×     |
| _WIN32 | ×     | ×              | √     |
| _WIN64 | ×     | ×              | x64   |

3.编译器的版本

用_MSC_VER定义编译器的版本。下面是一些编译器版本的_MSC_VER值：

MS VC++ 14.0 _MSC_VER = 1900

MS VC++ 10.0 _MSC_VER = 1600
MS VC++ 9.0 _MSC_VER = 1500
MS VC++ 8.0 _MSC_VER = 1400

其中MS VC++ 10.0就是Visual C++ 2010，MS VC++ 9.0就是Visual C++ 2008，MS VC++ 8.0就是Visual C++ 2005。