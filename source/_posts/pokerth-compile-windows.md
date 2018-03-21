---
title: pokerth在MXE下编译windows可执行程序
date: 2018-03-21 21:51:41
tags: [pokerth]
categories: [pokerth]
---

pokerth在MXE下编译windows可执行程序<!-- more -->, ubuntu14.04:

```shell
sudo apt-get install autoconf automake autopoint bash bison bzip2 flex gettext git g++ \
gperf intltool libffi-dev libgdk-pixbuf2.0-dev  libtool libltdl-dev libssl-dev \
libxml-parser-perl make openssl p7zip-full patch perl pkg-config python ruby scons \
sed unzip wget xz-utils

#On 64-bit Debian:
sudo apt-get install g++-multilib libc6-dev-i386
#On Debian 8 or Ubuntu 14.10 (or later)
sudo apt-get install libtool-bin

cd /opt
git clone https://github.com/mxe/mxe.git mingw ## ./mingw folder created
cd mingw
sudo make gcc 
sudo make qt ## takes a long time, over 1h (QT 4.X)
sudo make qt5 ## optional, only necessary if you are going to build with QT5. It takes ever more time than QT. Be patient xD 
sudo make boost
sudo make curl
sudo make libgsasl
sudo make sdl_mixer
sudo make tinyxml
sudo make tinyxml2
sudo make libircclient
sudo make protobuf

git clone https://github.com/pokerth/pokerth.git
cd pokerth
git checkout stable

/opt/mingw/usr/x86_64-pc-linux-gnu/bin/protoc -I=/home/jason/workspace/pokerth/ --cpp_out=/home/jason/workspace/pokerth/src/third_party/protobuf/ /home/jason/workspace/pokerth/pokerth.proto /home/jason/workspace/pokerth/chatcleaner.proto

#/opt/mingw/usr/x86_64-pc-linux-gnu/bin/protoc -I=/home/jason/workspace/pokerth-1.1.2-rc/ --cpp_out=/home/jason/workspace/pokerth-1.1.2-rc/src/third_party/protobuf/ /home/jason/workspace/pokerth-1.1.2-rc/pokerth.proto /home/jason/workspace/pokerth-1.1.2-rc/chatcleaner.proto

#export PATH=/opt/mingw/usr/x86_64-pc-linux-gnu/bin:/opt/mingw/usr/bin:$PATH
export PATH=/opt/mingw/usr/bin:$PATH

i686-w64-mingw32.static-qmake-qt4 CONFIG+="client" DEFINES+="BOOST_THREAD_USE_LIB" pokerth.pro  
make release

i686-w64-mingw32.static-qmake-qt5 CONFIG+="client" DEFINES+="BOOST_THREAD_USE_LIB" pokerth.pro
make release
```

