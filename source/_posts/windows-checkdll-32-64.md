---
title: windows检查dll是否64位库
date: 2018-02-01 22:31:44
tags: [windows,msvc编译]
categories: [贴士]
---

windows下有时候需要检查dll和lib是32位还是64位<!-- more -->，这里有一个简单工具，在VSdevcmd

下可以使用。

工具：dumpbin.exe 
依赖：link.exe, mspdb100.dll

命令： 
dumpbin /headers E:\math.dll

结果： 
Dump of file E:\math.dll 
PE signature found 
File Type: DLL

FILE HEADER VALUES 
14C machine (x86) ———————————————————>>>> x86为32位，x64为64位 
5 number of sections 
3CD30F99 time date stamp Sat May 04 06:30:49 2002 
0 file pointer to symbol table 
0 number of symbols 
E0 size of optional header 
210E characteristics 
Executable 
Line numbers stripped 
Symbols stripped 
32 bit word machine 
DLL