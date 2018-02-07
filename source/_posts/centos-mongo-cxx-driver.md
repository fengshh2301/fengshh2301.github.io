---
title: centos下编译mongo_cxx_driver
date: 2018-02-07 23:21:56
tags: [centos,mongodb]
categories: [贴士]
---

由于需要跨平台使用mongodb，需要编译官方的cxx driver<!-- more --> ，记录下centos7下的编译过程。

编译libbson

```shell
git clone https://github.com/mongodb/libbson.git
cd libbson/
git checkout -b 1.8.2 1.8.2
cmake .
make
sudo make install
```

编译mongo c driver

```shell
git clone https://github.com/mongodb/mongo-c-driver.git
cd mongo-c-driver/build
git checkout -b 1.8.2 1.8.2
./autogen.sh --with-libbson=bundled --enable-static
make
sudo make install
```

编译mongo cxx driver

```shell
git clone https://github.com/mongodb/mongo-cxx-driver.git
cd mongo-cxx-driver/
git checkout -b r3.2.0-rc1 r3.2.0-rc1
cd build/
#cmake -DCMAKE_BUILD_TYPE=Release BUILD_STATIC_LIBS=ON DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=/usr/local ..
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..
sudo make EP_mnmlstc_core
make
sudo make install
```

后记

这里还有一点需要说明，原本是希望使用静态库的，可是应用程序在链接mongo-cxx-driver静态库时，总是报：

```c++
undefined reference to `bsoncxx::v_noabi::builder::core::key_view(core::v1::basic_string_view<char, std::char_traits<char> >)'
```

暂时还没有找到原因，探索中。。。