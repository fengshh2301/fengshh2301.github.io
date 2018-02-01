---
title: windows.h定义的min和max宏相冲突
date: 2018-02-02 01:15:52
tags: [max宏]
categories: [贴士]
---

使用max，遇到这么个问题<!-- more -->

#### 错误输出

​    warning C4003: “max”宏的实参不足
​    error C2146: 语法错误: 缺少“)”(在标识符“max”的前面)
​    error C3646: “max”: 未知重写说明符

#### 错误原因

​    函数模板max与Visual C++中的全局的宏max冲突。

#### 解决办法

​    第一种办法(推荐)：添加宏定义NOMINMAX。

​    项目属性   ——> C/C++ ——> 预处理器 ——> 预处理器定义 (添加预定义编译开关NOMINMAX）

​    第二种办法： 加上括号，与Vsual C++的min/max宏定义区分开

​    int a = (std::max)(b, c);   