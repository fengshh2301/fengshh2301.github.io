---
title: centos下编译安装gcc4.9.3
date: 2018-03-18 17:17:00
tags: [centos,编译,gcc]
categories: [贴士]
---

今天想研究一个开源项目PokerTH <!-- more -->，先运行官网给的运行包，给我一堆这样的报错：

> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth)
> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./libs/libprotobuf.so.10)
> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./libs/libboost_filesystem.so.1.55.0)
> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./libs/libboost_regex.so.1.55.0)
> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./libs/libicui18n.so.52)
> /home/jason/workspace/PokerTH-1.1.2-linux/./pokerth: /lib64/libstdc++.so.6: version `CXXABI_1.3.8' not found (required by /home/jason/workspace/PokerTH-1.1.2-linux/./libs/libicuuc.so.52)

google下，发现是gcc不够新，好，升级。

## 源码

在https://ftp.gnu.org/gnu/下载源码包到机器上并解压：

```shell
tar -zxvf gcc-4.9.3.tar.gz
```



## 配置

```
 % mkdir build
 % cd build
 % srcdir/configure [options] [target]

```

在gcc-4.9.3目录里建立build目录，并在这个目录里执行configure。

configure官方文档<https://gcc.gnu.org/install/configure.html>

这里需要用c++，执行:

**~/download/gcc-4.9.3/configure --enable-languages=c,c++**

第一个问题：

### Issue 1

> configure: error: Building GCC requires GMP 4.2+, MPFR 2.4.0+ and MPC 0.8.0+. Try the --with-gmp, --with-mpfr and/or --with-mpc options to specify their locations. Source code for these libraries can be found at their respective hosting sites as well as at <ftp://gcc.gnu.org/pub/gcc/infrastructure/>. See also <http://gcc.gnu.org/install/prerequisites.html> for additional info. If you obtained GMP, MPFR and/or MPC from a vendor distribution package, make sure that you have installed both the libraries and the header files. They may be located in separate packages.

说是需要GMP 4.2+、MPFR 2.4.0+、MPC 0.8.0+。

查阅<https://gcc.gnu.org/install/prerequisites.html>：

> Several support libraries are necessary to build GCC, some are required, others optional. While any sufficiently new version of required tools usually work, library requirements are generally stricter. Newer versions may work in some cases, but it's safer to use the exact versions documented. We appreciate bug reports about problems with newer versions, though. If your OS vendor provides packages for the support libraries then using those packages may be the simplest way to install the libraries.
>
> GNU Multiple Precision Library (GMP) version 4.3.2 (or later)
>
> MPFR Library version 2.4.2 (or later)
>
> MPC Library version 0.8.1 (or later)
>
> ISL Library version 0.15, 0.14, 0.13, or 0.12.2

这里有个解决办法，在gcc-4.9.3目录执行:

**sh ./contrib/download_prerequisites**

依赖库下载好了，搞定。

### Issue 2

回到objdir目录，执行:

**../configure --enable-languages=c,c++**

warning：

> configure: WARNING: using in-tree ISL, disabling version check
>
> configure: WARNING: using in-tree CLooG, disabling version check
>
> *** This configuration is not supported in the following subdirectories: gnattools target-libada target-libgfortran target-libgo target-libffi target-libbacktrace target-zlib target-libjava target-libobjc target-boehm-gc (Any other directories should still work fine.)

这些目录是什么东西，懒得了解了，只是warning的话先不管了。重点是还出现以下的error提示：

> /usr/bin/ld: cannot find crt1.o: No such file or directory
>
> /usr/bin/ld: cannot find crti.o: No such file or directory
>
> /usr/bin/ld: skipping incompatible /usr/local/lib/gcc/x86_64-unknown-linux-gnu/4.9.3/libgcc.a when searching for -lgcc
>
> /usr/bin/ld: cannot find -lgcc
>
> /usr/bin/ld: cannot find -lgcc_s
>
> /usr/bin/ld: cannot find -lc
>
> /usr/bin/ld: skipping incompatible /usr/local/lib/gcc/x86_64-unknown-linux-gnu/4.9.3/libgcc.a when searching for -lgcc
>
> /usr/bin/ld: cannot find -lgcc
>
> /usr/bin/ld: cannot find -lgcc_s
>
> /usr/bin/ld: cannot find crtn.o: No such file or directory
>
> collect2: error: ld returned 1 exit status
>
> configure: error: I suspect your system does not have 32-bit developement libraries (libc and headers). If you have them, rerun configure with --enable-multilib. If you do not have them, and want to build a 64-bit-only compiler, rerun configure with --disable-multilib.

编译配置是默认支持32位和64位，但是32位的dev lib不齐全，建议关掉32位

**~/download/gcc-4.9.3/configure --disable-multilib --enable-languages=c,c++**

然后就天晴了，那些cannot find和error不见了（除了那个warning），没有其他异常提示：

> configure: creating ./config.status
>
> config.status: creating Makefile

## 编译

在objdir，执行:

```shell
make
```

经过漫长的等待，终于，编译成功。

```shell
make install
```

安装新的gcc。

## 配置libstdc++.so.6

```shell
ll /usr/lib64/libstdc++.so.6
```

> lrwxrwxrwx. 1 root root 19 Mar 18 23:15 /usr/lib64/libstdc++.so.6 -> libstdc++.so.6.0.19

```shell
cd build
find . -name libstdc++.so.* |xargs ls -l
```

> lrwxrwxrwx. 1 jason jason      19 Mar 19 01:53 ./prev-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6 -> libstdc++.so.6.0.20
> -rwxrwxr-x. 1 jason jason 6721712 Mar 19 01:53 ./prev-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.20
> lrwxrwxrwx. 1 jason jason      19 Mar 19 01:21 ./stage1-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6 -> libstdc++.so.6.0.20
> -rwxrwxr-x. 1 jason jason 6721712 Mar 19 01:21 ./stage1-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.20
> lrwxrwxrwx. 1 jason jason      19 Mar 19 02:04 ./x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6 -> libstdc++.so.6.0.20
> -rwxrwxr-x. 1 jason jason 6721712 Mar 19 02:04 ./x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.20

```shell
cp build/x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.20 /usr/lib64/
ldconfig
ll /usr/lib64/libstdc++.so.6
```

> lrwxrwxrwx. 1 root root 19 Nov 20 16:12 /usr/lib64/libstdc++.so.6 -> libstdc++.so.6.0.20