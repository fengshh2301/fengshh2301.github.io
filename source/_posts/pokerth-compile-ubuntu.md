---
title: pokerth在ubuntu下编译
date: 2018-03-21 21:51:49
tags: [pokerth]
categories: [pokerth]
---

pokerth在ubuntu14.04下编译:

#### It should compile with:

Compiler: GCC 4.8+, 5.x, 6.x (see [* KNOWN BUGS](https://github.com/pokerth/pokerth/wiki/Building-PokerTH-for-Linux/_edit#known-bugs)), 7.X (see [* KNOWN BUGS](https://github.com/pokerth/pokerth/wiki/Building-PokerTH-for-Linux/_edit#known-bugs))

Mode: c++98, c++11

QT: 4.x (>= 4.8), 5.x

#### Requirements

- qmake (Qt development tools): 4.8+. (current and recommended is 5.8: <https://www.qt.io/download-open-source/>)

  sudo apt-get install qt-sdk

  sudo apt-get install qt5-qmake

- boost 1.60, latest recommended (requires libbz2) (current and recommended is 1.63 <http://www.boost.org/users/history/version_1_63_0.html>).

  自编译：

  ```shell
  ./bootstrap.sh
  ./b2
  sudo ./b2 install
  ```

  ​

- libcurl4-gnutls

- libtinyxml

- libircclient

- libgsasl (at least 1.4)

- libsqlite3

- libgcrypt (should already be there as dependency to the previous libs)

- zlib version >= 1.2.3 --> <http://www.zlib.net/>

- libSDL_mixer, libSDL --> <http://www.libsdl.org/>

- libSQLite3 --> <http://sqlite.org/>

- protoc >= 2.3.0 (during build), libprotobuf >= 2.3.0 -> <https://github.com/google/protobuf>

  自编译：

  ```shell
  sudo apt-get install autoconf automake libtool curl make g++ unzip
  ./autogen.sh
  ./configure
  make
  make check
  sudo make install
  sudo ldconfig # refresh shared library cache.
  ```

  ​

- libmysql++, for official server

  ```shell
  sudo apt-get install libmysql++
  ```

  ​



