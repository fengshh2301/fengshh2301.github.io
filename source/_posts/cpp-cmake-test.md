---
title: 使用cmake编译cpp程序
date: 2020-12-30 13:33:39
tags: [cpp,C++,cmake]
categories: [工具]
---

鉴于cmake的简单易用<!-- more -->，后续开发尽量使用cmake来构建cpp程序。

#### 1)编译安装cmake

```bash
wget https://cmake.org/files/v3.16/cmake-3.16.8-Linux-x86_64.tar.gz
tar -zxvf cmake-3.16.8-Linux-x86_64.tar.gz
cd cmake-3.16.8-Linux-x86_64
./bootstrap --prefix=/usr/local/cmake
make
make install
ln -s /usr/local/cmake/bin/cmake /usr/bin/cmake
```

#### 2)使用cmake构建执行程序

##### a.简单示例

创建工程目录，以及程序代码目录

```bash
mkdir cmaketest
cd cmaketest
mkdir hello
cd hello
mkdir include #头文件目录
mkdir src #源文件目录
touch CMakeLists.txt #cmake文件
```

hello/src/main.cpp：

```c++
#include <iostream>

int main(int argc, char** args)
{
    std::cout << "hello world!" << std::endl;
    return 0;
}
```

hello/CMakeLists.txt：

```cmake
cmake_minimum_required(VERSION 2.8)

project(hello)

add_executable(${PROJECT_NAME}
    ./src/main.cpp
)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../bin)
```

cmaketest目录下创建build目录（后面构建使用，可以将构建的临时文件集中管理）

```bash
cd build
cmake ..
make
```

cmaketest目录下生成bin/hello，执行此二进制程序

```bash
bin/hello
hello world!
```

##### b.引用工程代码示例

增加两个文件：include/add.h，src/add.cpp。在其中实现一个求和函数add，供main.cpp调用。

hello/include/add.h代码：

```c++
#ifndef ADD_H
#define ADD_H

int add(int a, int b);

#endif // !ADD_H
```

hello/src/add.cpp代码：

```c++
#include <iostream>
#include "add.h"

int add(int a, int b) 
{
    std::cout << "this is func add." << std::endl;
	return a + b;
}
```

hello/src/main.cpp代码：

```c++
#include <iostream>
#include "add.h"

int main(int argc, char** args)
{
    std::cout << "hello world!" << std::endl;
	std::cout << "5+6=" << add(5, 6) << std::endl;
    return 0;
}
```

hello/CMakeLists.txt文件内容：

```cmake
cmake_minimum_required(VERSION 2.8)

project(hello)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add.cpp
)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../bin)
```

此时代码结构如下：

```bash
.
└── hello
    ├── CMakeLists.txt
    ├── include
    │   └── add.h
    └── src
        ├── add.cpp
        └── main.cpp

```



#### 3)使用cmake构建静态库并调用

创建一个静态库供hello程序调用

```bash
cd cmaketest
mkdir add_static
cd add_static
mkdir include #头文件目录
touch include/add_st.h
mkdir src #源文件目录
touch src/add_st.cpp
touch CMakeLists.txt #cmake文件
```

add_static/include/add_st.h代码：

```c++
#ifndef ADD_ST_H
#define ADD_ST_H

int add_st_func(int a, int b);

#endif // !ADD_ST_H
```

add_static/src/add_st.cpp代码：

```c++
#include <iostream>
#include "add_st.h"

int add_st_func(int a, int b)
{
    std::cout << "this is add_st_func." << std::endl;
	return a + b;
}
```

add_static/CMakeLists.txt文件内容：

```cmake
cmake_minimum_required(VERSION 2.8)

project(add_static)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library(${PROJECT_NAME} STATIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add_st.cpp
)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../lib)
```

hello/src/main.cpp代码：

```c++
#include <iostream>
#include "add.h"
#include "add_st.h"

int main(int argc, char** args)
{
    std::cout << "hello world!" << std::endl;
	std::cout << "5+6=" << add(5, 6) << std::endl;
	std::cout << "52+36=" << add_st_func(52, 36) << std::endl;
    return 0;
}
```

hello/CMakeLists.txt文件内容：

```cmake
cmake_minimum_required(VERSION 2.8)

project(hello)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../add_static/include
)

link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add.cpp
)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../bin)

target_link_libraries(${PROJECT_NAME}
    add_static
)
```

此时代码结构如下：

```bash
.
├── add_static
│   ├── CMakeLists.txt
│   ├── include
│   │   └── add_st.h
│   └── src
│       └── add_st.cpp
└── hello
    ├── CMakeLists.txt
    ├── include
    │   └── add.h
    └── src
        ├── add.cpp
        └── main.cpp

```

分别构建静态库add_static和hello程序之后，cmaketest目录下生成bin/hello，执行此程序

```bash
bin/hello
hello world!
this is func add.
5+6=11
this is add_st_func.
52+36=88
```



#### 4)使用cmake构建动态库并调用

创建一个动态库供hello程序调用

```bash
cd cmaketest
mkdir add_dynamic
cd add_dynamic
mkdir include #头文件目录
touch include/add_dy.h
mkdir src #源文件目录
touch src/add_dy.cpp
touch CMakeLists.txt #cmake文件
```

add_dynamic/include/add_dy.h代码：

```c++
#ifndef ADD_DY_H
#define ADD_DY_H

int add_dy_func(int a, int b);

#endif // !ADD_DY_H
```

add_dynamic/src/add_dy.cpp代码：

```c++
#include <iostream>
#include "add_dy.h"

int add_dy_func(int a, int b)
{
	std::cout << "this is add_dy_func." << std::endl;
	return a + b;
}
```

add_dynamic/CMakeLists.txt文件内容：

```cmake
cmake_minimum_required(VERSION 2.8)

project(add_dynamic)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library(${PROJECT_NAME} SHARED
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add_dy.cpp
)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../lib)
```

hello/src/main.cpp代码：

```c++
#include <iostream>
#include "add.h"
#include "add_st.h"
#include "add_dy.h"

int main(int argc, char** args)
{
    std::cout << "hello world!" << std::endl;
	std::cout << "5+6=" << add(5, 6) << std::endl;
	std::cout << "52+36=" << add_st_func(52, 36) << std::endl;
	std::cout << "2+33=" << add_dy_func(2, 33) << std::endl;
    return 0;
}
```

hello/CMakeLists.txt文件内容：

```cmake
cmake_minimum_required(VERSION 2.8)

project(hello)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../add_static/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../add_dynamic/include
)

link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add.cpp
)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../bin)

target_link_libraries(${PROJECT_NAME}
    add_static
    add_dynamic
)

```

此时代码结构如下：

```bash
.
├── add_dynamic
│   ├── CMakeLists.txt
│   ├── include
│   │   └── add_dy.h
│   └── src
│       └── add_dy.cpp
├── add_static
│   ├── CMakeLists.txt
│   ├── include
│   │   └── add_st.h
│   └── src
│       └── add_st.cpp
└── hello
    ├── CMakeLists.txt
    ├── include
    │   └── add.h
    └── src
        ├── add.cpp
        └── main.cpp

```

分别构建静态库add_static、动态库add_dynamic和hello程序之后，cmaketest目录下生成lib/libadd_static.a、lib/libadd_dynamic.so、bin/hello，执行hello

```bash
bin/hello
hello world!
this is func add.
5+6=11
this is add_st_func.
52+36=88
this is add_dy_func.
2+33=35
```



#### 5)add_subdirectory使用

从上面使用可以看到，三个目标的构建彼此分离，单独操作，有诸多不便。此时可以创建cmaketest/CMakeLists.txt：

```cmake
cmake_minimum_required(VERSION 2.8)

project(cmaketest)

add_subdirectory(add_dynamic add_dyn)
add_subdirectory(add_static add_sta)
add_subdirectory(hello hellotest)

```

后面编译就方便多了。

```bash
cd cmaketest
mkdir build
cmake ..
make
```



#### 6)使用cmake跨平台构建简介

上述步骤操作下来，linux系统可以正常构建。但windows下动态库配套的接口库没有生成。

这里需要做些修改。

add_dynamic/include/add_dy.h代码：

```c++
#ifndef ADD_DY_H
#define ADD_DY_H

#if(defined WIN32||defined_WIN32|| defined WINCE)&& defined EFIMPL
#define EXPORT __declspec(dllexport)
#else
#define EXPORT
#endif

EXPORT int add_dy_func(int a, int b);

#endif // !ADD_DY_H
```

add_dynamic/CMakeLists.txt：

```cmake
cmake_minimum_required(VERSION 2.8)

project(add_dynamic)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library(${PROJECT_NAME} SHARED
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add_dy.cpp
)

#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../lib)
if(MSVC)
add_definitions(
    -DEFIMPL
)
endif()

set_target_properties(${PROJECT_NAME} PROPERTIES
                      VERSION 1.0
                      SOVERSION 1
                      OUTPUT_NAME ${PROJECT_NAME}
                      CLEAN_DIRECT_OUTPUT 1
                      RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../lib
                      ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../lib
)

```

为风格统一，各CMakeLists.txt文件使用set_target_properties。

add_static/CMakeLists.txt：

```cmake
cmake_minimum_required(VERSION 2.8)

project(add_static)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library(${PROJECT_NAME} STATIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add_st.cpp
)

#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../lib)

set_target_properties(${PROJECT_NAME} PROPERTIES
                        OUTPUT_NAME ${PROJECT_NAME}
                        CLEAN_DIRECT_OUTPUT 1
                        ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../lib
)

```

hello/CMakeLists.txt：

```cmake
cmake_minimum_required(VERSION 2.8)

project(hello)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../add_static/include
    ${CMAKE_CURRENT_SOURCE_DIR}/../add_dynamic/include
)

link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../lib
)

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/add.cpp
)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/../bin)

target_link_libraries(${PROJECT_NAME}
    add_static
    add_dynamic
)

set_target_properties(${PROJECT_NAME} PROPERTIES
                      OUTPUT_NAME ${PROJECT_NAME}
                      CLEAN_DIRECT_OUTPUT 1
                      RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../bin
)

```

